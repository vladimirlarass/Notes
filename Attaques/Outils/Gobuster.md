Gobuster est un outil offensif open source écrit en Golang. Il énumère les répertoires web, les sous-domaines DNS , les hôtes virtuels, les buckets Amazon S3 et Google Cloud Storage par force brute, en utilisant des listes de mots spécifiques et en gérant les réponses entrantes. De nombreux professionnels de la sécurité utilisent cet outil pour les tests d'intrusion, la chasse aux bugs et les évaluations de cybersécurité. Si l'on considère les phases du piratage éthique, on peut placer Gobuster entre les phases de reconnaissance et d'analyse.

## énumération de répertoires et de fichiers
Gobuster dispose d'un `dir`mode permettant aux utilisateurs d'énumérer les répertoires de sites web et leurs fichiers.
Pour exécuter Gobuster en `dir`mode, utilisez le format de commande suivant :

```Bash
gobuster dir -u "http://www.example.thm" -w /path/to/wordlist
```

Regardons un deuxième exemple où nous utilisons l’ `-x`indicateur pour spécifier le type de fichiers que nous voulons énumérer :

```Bash
gobuster dir -u "http://www.example.thm" -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x .php,.js
```

commande pour ignorer la vérification TLS

```
--no-tls-validation
```

## énumération de sous-domaines
Le mode suivant que nous allons examiner est le `dns`mode. Ce mode permet à Gobuster d'attaquer les sous-domaines par force brute. Lors d'un test d'intrusion, il est essentiel de vérifier les sous-domaines du domaine principal de votre cible.
Pour exécuter Gobuster en mode DNS , utilisez la syntaxe de commande suivante :  

```Bash
gobuster dns -d example.thm -w /path/to/wordlist
```

## Enumération Vhost

Ce mode permet à Gobuster d'attaquer par force brute les hôtes virtuels. Les hôtes virtuels sont des sites web différents sur la même machine. Ils ressemblent parfois à des sous-domaines, mais ne vous y trompez pas ! Les hôtes virtuels sont basés sur une adresse IP et fonctionnent sur le même serveur.
Pour exécuter Gobuster en `vhost`mode, tapez la commande suivante :

```bash
gobuster vhost -u "http://example.thm" -w /path/to/wordlist
```


```
gobuster vhost -u http://heal.htb -w /usr/share/wordlists/dirb/small.txt -k --append-domain
```

```
gobuster dir -u http://10.10.11.48 -w /usr/share/wordlists/seclists/Discovery/DNS/dns-Jhaddix.txt
```
