# Enumeration 
# Nmap 
Nous avons débuté l'engagement par une analyse de port à l'aide de l'outil Nmap afin d'identifier les services exposés sur la cible.

![[Pasted image 20250512044623.png]]

Les résultats du scan Nmap indiquent que seuls les ports 22 (SSH) et 80 (HTTP) sont ouverts sur la cible.

Lors de l'analyse, nous avons identifié le domaine `planning.htb`, qu’il a été nécessaire d’ajouter manuellement au fichier `hosts` local afin d’en permettre la résolution DNS.

![[Pasted image 20250512044852.png]]



![[Pasted image 20250512051710.png]]

on a les informations d'authentification fournit pas htb 

![[Pasted image 20250512052737.png]]


nous avons trouvez la version de grafana  qui est la version 11.0.0 nous allons recherchez s'il y a une vulnerabilite que nous pouvons exploiter 

![[Pasted image 20250512053901.png]]

nous avons trouve un exploit 
https://github.com/z3k0sec/CVE-2024-9264-RCE-Exploit

![[Pasted image 20250512060411.png]]


![[Pasted image 20250512060301.png]]

maintenant regardons 'env ' et nous avons trouve un login et un mot de passe 
login : enzo
password : RioTecRANDEntANT!

![[Pasted image 20250512060532.png]] essayons maintenant de nous connecter en ssh avec ces informations 


![[Pasted image 20250512060740.png]]

![[Pasted image 20250512060835.png]]
 Root 
P4ssw0rdS0pRi0T3c

![[Pasted image 20250521171535.png]]
reverse shell 
./bash -p