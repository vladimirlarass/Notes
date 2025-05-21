Powershell est le langage de script Windows et l'environnement shell construit à l'aide du framework .NET.

# Commande PowerShell de base 
## Utilisation de Get-Help

`Get-Help`Affiche des informations sur une _applet de commande._ Pour obtenir de l'aide sur une commande spécifique, exécutez la commande suivante :
```Powershell
Get-Help Command-Name
```

Vous pouvez également comprendre comment utiliser la commande en lui transmettant l' `-examples`indicateur. 

## Utilisation de Get-Command

`Get-Command`Récupère toutes les _applets de commande_ installées sur l'ordinateur actuel. L'avantage de cette _applet de commande_  est qu'elle permet des correspondances de motifs comme celles-ci.

`Get-Command Verb-*`ou`Get-Command *-Noun`

L'exécution `Get-Command New-*`pour afficher toutes les _applets de commande_  pour le verbe new .

## Le pipeline (|)

Le pipeline ( `|`) permet de transmettre la sortie d'une _applet de commande_  à une autre. Une différence majeure par rapport aux autres shells réside dans le fait que Powershell transmet un objet à l' _applet de commande_ suivante au lieu de transmettre du texte ou une chaîne à la commande après le pipeline.

Un exemple d'exécution de cette fonction pour afficher les membres `Get-Command`est :

```PowerShell
Get-Command | Get-Member -MemberType Method
```

## _Création d'objets à partir d'applets de commande_ précédentes

Une façon de manipuler des objets consiste à extraire les propriétés de la sortie d'une _applet de commande_ et à créer un nouvel objet. Cette opération s'effectue à l'aide de l' `Select-Object` _applet de commande._
Voici un exemple de liste des répertoires et de sélection du mode et du nom :

```PowerShell
Get-ChildItem | Select-Object -Property Mode, Name
```

## Filtrage des objets

Lors de la récupération d'objets de sortie, vous souhaiterez peut-être sélectionner des objets correspondant à une valeur très spécifique. Pour ce faire, utilisez le `Where-Object`filtre basé sur la valeur des propriétés.

Le format général d'utilisation de cette _applet de commande_  est 

```Powershell
Verb-Noun | Where-Object -Property PropertyName -operator Value

Verb-Noun | Where-Object {$_.PropertyName -operator Value}
```

Où `-operator`se trouve la liste des opérateurs suivants :

- `-Contains`: si un élément de la valeur de la propriété correspond exactement à la valeur spécifiée
- `-EQ`: si la valeur de la propriété est la même que la valeur spécifiée
- `-GT`: si la valeur de la propriété est supérieure à la valeur spécifiée

## Trier l'objet
Lorsqu'une _applet de commande_ génère beaucoup d'informations, vous devrez peut-être les trier pour les extraire plus efficacement. Pour ce faire, transférez la sortie d'une _applet de commande_  vers l' `Sort-Object` _applet de commande_ .
Le format de la commande serait :

```PowerShell
Verb-Noun | Sort-Object
```


## Quelques Commande 

Rechercher un fichier par nom dans un dossier et ses sous-dossiers

```PowerShell
Get-ChildItem -Path "C:\Users\Michel\Documents" -Recurse -Filter "rapport.docx"
```

La commande pour obtenir le répertoire de travail actuel 

```Powershell
Get-Location
```

La commande  pour faire une requête à un serveur Web 

```powerShell
Invoke-WebRequest
```
# Enumeration 


Decouvrir le nombre utilisateurs sur la machine 

```PowerShell
Get-LocalUser
```

obtenir des **informations détaillées sur tous les comptes utilisateur** du système (y compris locaux **et** de domaine, s'il est joint à un domaine)

```PowerShell
Get-WmiObject -Class Win32_UserAccount
```

La commande  utilisée pour obtenir les informations d'adresse IP 

```PowerShell
Get-NetIPAddress
```

la commande pour répertorier les  ports étant à l'écoute 

```PowerShell
Get-NetTCPConnection
```

La commande  pour lister tous les processus en cours d'exécution 

```PowerShell
Get-Process
```

la Commande pour tous les taches planifiées 

```PowerShell
Get-ScheduledTask
```

récupère la liste de contrôle d'accès (ACL) pour le chemin spécifié.

```PowerShell
Get-Acl -Path "C:\chemin\du\dossier-ou-fichier"
```


https://learnxinyminutes.com/powershell/
https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/where-object?view=powershell-7.5&viewFallbackFrom=powershell-6