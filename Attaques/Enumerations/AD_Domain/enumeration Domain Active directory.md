
**Le domaine** est une unité fondamentale d'Active Directory . Un domaine AD est un groupe logique d'objets qui partagent des paramètres d'administration, de sécurité et de réplication communs. Grâce aux domaines Active Directory, les équipes informatiques peuvent définir des limites administratives et gérer des ensembles d'appareils, de services et de systèmes de manière centralisée.

Pour enumeration de domaine d'active directory nous pouvons utiliser les outils suivant : 
- Module Active directory de PowerShell 
```
 Import-Module C:\AD\Tools\ADModule-
master\ActiveDirectory\ActiveDirectory.psd1
```

```
Import-Module C:\AD\Tools\ADModule master\Microsoft.ActiveDirectory.Management.dll
Import-Module C:\AD\Tools\ADModule-master\ActiveDirectory\ActiveDirectory.psd1
```

```
Get-ADDomain 
Get-ADUser
```


- [[Bloodhound]] C# and PowerShell Collectors)

- [[Poweview]] ( PowerShell) 
- SharpView (C#) - Doesn't support filtering using Pipeline

https://learn.microsoft.com/en-us/powershell/module/activedirectory/?view=windowsserver2022-ps
