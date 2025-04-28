`Domain Name System`( `DNS`) fait partie intégrante d'Internet.
/home/kali/Documents/TryHackme/target_HTB_NFS

L' `SOA`enregistrement se trouve dans le fichier de zone d'un domaine et spécifie qui est responsable du fonctionnement du domaine et comment les informations DNS du domaine sont gérées.

```shell-session
dig soa www.inlanefreight.com
```

Le point (.) est remplacé par un arobase (@) dans l'adresse e-mail. 

il est parfois possible d'interroger la version d'un serveur DNS à l'aide d'une requête de classe CHAOS et de type TXT.
```shell-session
dig CH TXT version.bind 10.129.120.85
```

Nous pouvons utiliser l'option `ANY`permettant d'afficher tous les enregistrements disponibles.

```shell-session
dig any inlanefreight.htb @10.129.14.128
```

`Zone transfer`désigne le transfert de zones vers un autre serveur DNS, généralement via le port TCP 53. Cette procédure est abrégée `Asynchronous Full Transfer Zone`( `AXFR`). Une panne DNS ayant généralement de graves conséquences pour une entreprise, le fichier de zone est presque toujours identique sur plusieurs serveurs de noms. Lors de modifications, il est nécessaire de s'assurer que tous les serveurs disposent des mêmes données. La synchronisation entre les serveurs concernés est réalisée par transfert de zone. À l'aide d'une clé secrète `rndc-key`, que nous avons initialement vue dans la configuration par défaut, les serveurs s'assurent de communiquer avec leur propre maître ou esclave. Le transfert de zone implique le simple transfert de fichiers ou d'enregistrements et la détection des divergences dans les ensembles de données des serveurs concernés.

```shell-session
dig axfr inlanefreight.htb @10.129.14.128
```

```shell-session
for sub in $(cat /opt/useful/seclists/Discovery/DNS/subdomains-top1million-110000.txt);do dig $sub.inlanefreight.htb @10.129.14.128 | grep -v ';\|SOA' | sed -r '/^\s*$/d' | grep $sub | tee -a subdomains.txt;done
```

De nombreux outils différents peuvent être utilisés à cette fin, et la plupart fonctionnent de la même manière. [DNSenum](https://github.com/fwaeytens/dnsenum) en est un exemple .

```shell-session
dnsenum --dnsserver 10.129.14.128 --enum -p 0 -s 0 -o subdomains.txt -f /opt/useful/seclists/Discovery/DNS/subdomains-top1million-110000.txt inlanefreight.htb
```
