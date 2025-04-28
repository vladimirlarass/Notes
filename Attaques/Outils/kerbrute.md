Kerbrute est un outil d'énumération populaire utilisé pour forcer et énumérer les utilisateurs Active Directory valides en abusant de la pré-authentification Kerberos .
 [https://github.com/ropnop/kerbrute/releases](https://github.com/ropnop/kerbrute/releases) 
Rendre Kerbrute executable 
Enumeration des utilisateurs 
L'énumération des utilisateurs vous permet de savoir quels comptes d'utilisateurs se trouvent sur le domaine cible et quels comptes pourraient potentiellement être utilisés pour accéder au réseau.

```
./kerbrute userenum --dc CONTROLLER.local -d CONTROLLER.local User.txt
```
 