# Get-DomainGPOLocalGroup

Renvoie tous les objets de stratégie de groupe d'un domaine qui modifient les appartenances aux groupes locaux via les préférences « Groupes restreints » ou Stratégie de groupe. Renvoie également leurs mappages d'appartenance d'utilisateur, s'ils existent.
# Get-DomainObjectAcl

Renvoie les ACL associées à un objet Active Directory spécifique.
debut de l'attaque [[Abusing Active Directory ACLs]]


```
Get-DomainObjectAcl -Identity "Domain Admins" -ResolveGUIDs -
Verbose
```

PowerView

- Le Domaine Courant 
Get-Netdomain
- Obtenir le SID du domain actuel 
Get-DomainSID
- Obtenir les COntrolleur de domaine 
Get-NetDomainController
- Obtenir la liste des utilisateurs du Domaine 
Get-NetUser
- Recherche les machines sur le domaine local où l'utilisateur actuel dispose d'un accès administrateur local.
```
Find-LocalAdminAccess
```
