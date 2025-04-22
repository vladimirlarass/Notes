Port 21  
 Quelques Commande FTP
 get  , Connect , put , quit , status , verbose 
  ft $IP
Telecharger tous les fichiers disponibles 
```
wget -m --no-passive ftp://anonymous:anonymous@10.129.14.13
```

Trouver tous les scripts ftp Nmap 

 ```
find / -type f -name ftp* 2>/dev/null | grep scripts
```

