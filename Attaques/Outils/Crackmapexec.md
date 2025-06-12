### enumeration utilisateur 
```bash
crackmapexec smb 192.168.215.104 -u 'user' -p 'PASS' --users
```



```
crackmapexec smb 10.211.11.20 -u listenom.txt -p passtest.txt
```

#### Connectez-vous à la cible à l'aide d'un compte local

```bash
crackmapexec smb 192.168.215.138 -u 'Administrator' -p 'PASSWORD' --local-auth

crackmapexec smb 172.16.157.0/24 -u administrator -H 'LMHASH:NTHASH' --local-auth
```

#### Enumerer les utilisateurs 

```bash
crackmapexec smb 192.168.215.104 -u 'user' -p 'PASS' --users
```

```bash
# Perform RID Bruteforce to get users
crackmapexec smb 192.168.215.104 -u 'user' -p 'PASS' --rid-brute

# Enumerate domain groups
crackmapexec smb 192.168.215.104 -u 'user' -p 'PASS' --groups

# Enumerate local users
crackmapexec smb 192.168.215.104 -u 'user' -p 'PASS' --local-users
```



https://cheatsheet.haax.fr/windows-systems/exploitation/crackmapexec/