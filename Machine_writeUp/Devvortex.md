Difficulte : Easy 

# Enumeration 
## Nmap 

Nous avons débuté l'engagement par une analyse de port à l'aide de l'outil Nmap afin d'identifier les services exposés sur la cible.

```
nmap -sV -sC 10.10.11.242 -v  --min-rate 1000 -Pn -T4 -p-
```

![[Pasted image 20250502140206.png]]
Les résultats du scan Nmap indiquent que seuls les ports 22 (SSH) et 80 (HTTP) sont ouverts sur la cible.

Lors de l'analyse, nous avons identifié le domaine `devvortex.htb`, qu’il a été nécessaire d’ajouter manuellement au fichier `hosts` local afin d’en permettre la résolution DNS.

![[Pasted image 20250502141426.png]]

Suite à l’énumération des sous-domaines et des répertoires à l’aide de l’outil FFUF, nous avons découvert un sous-domaine supplémentaire : `dev.devvortex.htb`.

![[Pasted image 20250502145756.png]]

L’énumération des domaines associés à ``dev.devvortex.htb`` nous a permis d’identifier une interface d’administration accessible via `dev.devvortex.htb/administrator`, sur laquelle nous avons constaté la présence d’un template Joomla.


![[Pasted image 20250502145834.png]]

Après avoir identifié l’utilisation du CMS Joomla, nous avons analysé les fichiers accessibles publiquement et inspecté le code source de la page d’administration. Cette investigation nous a permis de retrouver un fichier `joomla.xml`, situé dans `/administrator/manifests/files/`, contenant les métadonnées du CMS, notamment sa version exacte. Une vérification croisée via des recherches en ligne a confirmé la version identifiée.

![[Pasted image 20250502152002.png]]

Nous avons ensuite effectué des recherches en ligne concernant la version identifiée de Joomla et avons découvert qu’elle est affectée par une vulnérabilité connue et nous l'avions exploite 

```
ruby exploit.rb  http://dev.devvortex.htb
```

user : lewis
password: P4ntherg0t1n5r3c0n##

![[Pasted image 20250502152226.png]]


## Server-Side Template injection 

Après avoir accédé à l’interface d’administration Joomla, nous avons exploité une vulnérabilité de type Server-Side Template Injection (SSTI) présente dans le gestionnaire de templates. Cette faille nous a permis d’injecter du code côté serveur et d’obtenir une exécution de commandes arbitraires sur la machine cible.

![[Pasted image 20250502152500.png]]



![[Pasted image 20250502161145.png]]


![[Pasted image 20250502161313.png]]

Après l’exploitation initiale, nous avons poursuivi avec une phase d’énumération des services internes disponibles sur le réseau. Cette analyse nous a permis d’identifier un service MySQL en cours d’exécution. En nous connectant à ce service, nous avons pu énumérer les bases de données ainsi que leurs tables. Cette exploration nous a permis de récupérer des informations sensibles, notamment les identifiants associés à l’utilisateur 'logan'.

```
nc -lvnp 443 
```

```
$ ss -tlpn
```

![[Pasted image 20250502162814.png]]

![[Pasted image 20250502162900.png]]


![[Pasted image 20250502154246.png]]

$2y$10$6V52x.SD8Xc7hNlVwUTrI.ax4BIAYuhVBMVvnYWRceBmy8XdEzm1u


![[Pasted image 20250502154307.png]]

logan 
tequieromucho

Grâce à l'identification du hash bcrypt associé à l'utilisateur 'logan', nous avons réussi à retrouver le mot de passe en clair par attaque par dictionnaire. Cela nous a permis de nous connecter avec succès en ssh .

```
hashcat -m 3200 lol.txt /home/kali/Documents/TryHackme/Wordlist/rockyou.txt
```


![[Pasted image 20250502154441.png]]

# Elevation de privilege 

Dans le cadre de l’élévation de privilèges, nous avons analysé l’environnement du système compromis à la recherche de vulnérabilités locales. Cette analyse nous a permis d’identifier une faille de sécurité connue, référencée sous un identifiant CVE-2023-1326, exploitable sur le système. En tirant parti de cette vulnérabilité, nous avons obtenu un accès privilégié (root) à la machine


![[Pasted image 20250502154842.png]]

![[Pasted image 20250502160104.png]]




https://github.com/Acceis/exploit-CVE-2023-23752
https://github.com/diego-tella/CVE-2023-1326-PoC
https://x.com/7h3h4ckv157/status/1728753489839628604
https://www.revshells.com/
https://academy.hackthebox.com/module/details/145