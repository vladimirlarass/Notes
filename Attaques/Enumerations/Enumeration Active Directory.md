
### **1. Nom de domaine et forêts  

La commande pour obtenir le Domaine Actuel : 

```
Get-Domain (PowerView)
Get-ADDomain (ActiveDirectory Module)
```

Obtenir l'objet d'un autre Domaine 

```
Get-Domain -Domain example.local
Get-ADDomain -Identity example.local
```

Obtenir le [[SID ( Identificateurs de sécurité )]] du domaine actuel 
https://powersploit.readthedocs.io/en/latest/Recon/Get-DomainSID/

```
Get-DomainSID
(Get-ADDomain).DomainSID
```


Obtenir le [[contrôleur de Domaine]] pour le domaine actuel -
https://learn.microsoft.com/en-us/powershell/module/activedirectory/get-addomaincontroller?view=windowsserver2025-ps

```
Get-DomainContorller
Get-ADDomainController
```

Obtenir une liste de toutes les [[Relation D'approbation Pour les Forest Dans l'AD]] pour le domaine actuel -

```
Get-DomainTrust
Get-DomainTrust -Domain us.dollarcorp.moneycorp.local

Get-ADTrust
Get-ADTrust -Identity us.dollarcorp.moneycorp.local
```

Obtenez des détails sur la forêt actuelle -

```
Get-Forest
Get-Forest -Forest eurocorp.local

Get-ADForest
Get-ADForest -Indentiy eurocorp.local
```

Obtenir tous les domaines de la forêt actuelle -

```
Get-ForestDomain
Get-ForestDomain -Forest eurocorp.local

(Get-ADForest).Domains
```

Cartographier les fiducies d'une forêt -

```
Get-ForestTrust
Get-ForestTrust -Forest eurocorp.local

Get-ADTrust -Filter 'ms-DS-TrustForestTrustInfo -ne "$null"'
```

### **2. Utilisateurs**

Obtenir une liste des utilisateurs du domaine actuel -
https://powersploit.readthedocs.io/en/latest/Recon/Get-DomainUser/

```
Get-DomainUser
Get-DomainUser -Identity user1
Get-DomainUser | select SamAccountName

Get-ADUser -Filter * -Properties *
Get-ADUSer -Identity user1 -Properties *
```

Obtenir la liste de toutes les propriétés de l'utilisateur dans le domaine actuel -

```
Get-DomainUser -Identity user1 -Properties *
Get-DomainUser -Properties SamAccountName,logonCount

 Get-ADUser -Filter * -Properties * | select -First 1 | Get-Member -MemberType *Property | select Name
 Get-ADUser -Filter * -Properties * | select name,logonCount, @{expression={ []datetime::fromFileTime (&_.pwdlastset) }}
```

Obtenez tous les membres du groupe « Administrateurs d'entreprise » -

```
Get-DomainGroupMember -Identity "Enterprise Admins" -Domain example.local -Recurse
```

Répertoriez tous les groupes locaux sur la machine (nécessitant des privilèges d'administrateur sur la machine non-DC) -

```
Get-NetLocalGroup -ComputerName dcorp-dc
```

Obtenir les membres du groupe local « Administrateurs » sur une machine (nécessite des privilèges d'administrateur sur les machines non-DC) -


```
Get-NetLocalGroup -ComputerName dcorp-dc -GroupName Administrators
```

Rechercher toutes les machines du domaine actuel sur lesquelles l'utilisateur actuel dispose de l'accès administrateur local -

```
Find-LocalAdminAccess -Verbose ] --> this works like => Get-DomainComputer > Invoke-CheckLocalAdminAccess

or can use -->
Find-WMILocalAdminAccess.ps1 and Find-PSRemotingLocalAdminAccess.ps1 (scripts)
```

Comptes avec mots de passe n'expirant jamais

```
Get-ADUser -Filter {PasswordNeverExpires -eq $true} -Properties PasswordNeverExpires | Select SamAccount
```

Comptes avec délégation activée (Kerberos Delegation)

```
Get-ADUser -Filter {TrustedForDelegation -eq $true -or TrustedToAuthForDelegation -eq $
```

Pour vérifier les privilèges de l'utilisateur actuel, utilisez :

```powershell
$ whoami /priv
```

### **3. Groupes**

Obtenir tous les groupes du domaine actuel -

```
Get-DomainGroup | Select Name
Get-DomainGroup -Domain <targetdomin>

Get-ADGroup -Filter * | Select Name
Get-ADGroup -Filter * -Properties *
``` 

 Gelez tous les groupes contenant le mot « admin » dans le nom du groupe -

```
Get-DomainGroup *admin*

Get-ADGroup -Filter 'Name -Like "*admin*"' | Select Name
```

#### Obtenez tous les membres du groupe « Administrateurs du domaine » -

```
Get-DomainGroupMember -Identity "Domain Admins" -Recurse
Get-ADGroupMember -Identity "Domain Admins" -Recursive
```

#### Obtenez tous les membres du groupe « Administrateurs d'entreprise » -

```
Get-DomainGroupMember -Identity "Enterprise Admins" -Domain example.local -Recurse
```
### **4. Machines & Serveurs**

Obtenir une liste des ordinateurs dans un domaine actuel -

```
Get-DomainComputer | select Name
Get-DomainComputer -OperatingSystem "*Server 2022*"
Get-DomainComputer -Ping

Get-ADComputer -Filter * | Select Name
Get-ADComputer -FIlter * | -Properties *
Get-AdComputer -Filter 'OperatingSystem -Like "*Server 2022*"' -Properties OperatingSystem | Select name,OperatingSystem
GEt-ADComputer -Filter * -Properties DNSHostName | %{Test-Connection -Count 1 -ComputerName $_.DNSHostName}
```

Rechercher les ordinateurs sur lesquels un administrateur de domaine (ou un utilisateur/groupe spécifié) a des sessions -

```
Find-DomainUserLocation -Verbose
Find-DomainUserLocation -UserGroupIdentity "RDPUsers"

It works like -->
> Get-DomainGroupMember > Get-DomainComputer > Get-NetLoggedon
NOTE that server 2019 onwards, local administrator privileges are required to list sessions.
```

Rechercher les ordinateurs sur lesquels une session d'administrateur de domaine est disponible et l'utilisateur actuel dispose d'un accès administrateur (utilise Test-AdminAccess) -

```
Find-DomainUserLocation -CheckAccess
```

Rechercher des ordinateurs (serveurs de fichiers et serveurs de fichiers distribués) sur lesquels une session d'administrateur de domaine est disponible

```
Find-DomainUserLocation -Stealth
```

### **5. Politiques et Permissions**

Obtenir la [[Strategie de Groupe]] pour le domaine actuel -

```
Get-DomainPolicyData
(Get-DomainPolicyData).systemaccess
```

Obtenir la liste des GPO dans le domaine actuel -

```
Get-DomainGPO
Get-DomainGPO -ComputerName xcorp-user1
```

Obtenir les GPO qui utilisent des groupes restreints ou des groupes.xml pour les utilisateurs intéressants -


```
Get-DomainGPOLocalGroup
```

Obtenir les utilisateurs qui se trouvent dans un groupe local d'une machine à l'aide de GPO -

```
Get-DomainGPOComputerLocalGroupMapping -ComputerIdentity xcorp-user1
```

Obtenir les OU dans un domaine -

```
Get-DomainOU
Get-DomainOU | select Name

Get-ADOrganizationallUnit -Filter * -Properties *
```


### **6. ACL (Access Control List) et Permissions**

Obtenir les ACL associées à l'objet spécifié -

```
Get-DomainObkectAcl -SamAccountName user1 -ResloveGUIDs
```

Rechercher des ACE intéressants -

```
Find-InterestingDomainAcl -ResolveGUIDs

Find-InterestingDomainAcl -ResolveGUIDs | ?{$_.IdentityReferenceName -match "RDPUsers"} | select ObjectDN,ActiveDirectoryRights
```

Obtenir les ACL associées au chemin spécifié -

```
Get-PathAcl -Path "\\dcorp-dc.dollarcorp.moneycorp.local\sysvol"
```

### **7. Autres éléments techniques**

Obtenir tous les serveurs de fichiers du domaine -

```
Get-NetFileServer
```
