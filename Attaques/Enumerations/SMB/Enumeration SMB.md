# **Definition** 

Server Message Block ( SMB ) est un protocole de communication utilisé pour partager des fichiers, des imprimantes , des ports série et diverses communications entre les nœuds d'un réseau 




port SMB 
139 
445 
## tools 
smbclient 
 - L : list des partages sur la machines 
 - -U : user 
 smbmap 
 - H : user 
 -
 crackmapexec smb 192.168.1.17 -u 'raj' -p '123' --shares
 --users 
 ```
sudo nmap $ip$ -sV -sC -p139,445
```

```
rpcclient -U " " $ip
```

Le `rpcclient`client rpc propose de nombreuses requêtes permettant d'exécuter des fonctions spécifiques sur le serveur SMB pour obtenir des informations
srvinfo , enumdomains, querydominfo, netshareenumall , netsharegetinfo , enumdomusers , queryuser <RID>

Impacket   - Samrrdump.py

```
smbmap -H $ip
```

 
https://fr.wikipedia.org/wiki/Server_Message_Block