Il achemine tout le trafic provenant de n'importe quel outil de ligne de commande vers n'importe quel proxy que nous spécifions.
il ajoute un proxy à n'importe quel outil de ligne de commande et constitue donc la méthode la plus simple  pour acheminer le trafic Web des outils de ligne de commande via nos proxys Web.
Pour utiliser `proxychains`, nous devons d'abord éditer `/etc/proxychains.conf`, commenter la dernière ligne et ajouter la ligne suivante à la fin de celle-ci :

```shell-session
#socks4         127.0.0.1 9050
http 127.0.0.1 8080
```

Nous devrions également activer `Quiet Mode` pour la réduction du bruit en décommettant `quiet_mode`. Une fois cela fait, nous pouvons ajouter `proxychains` a n'importe quelle commande au début, et le trafic de cette commande sera acheminé via `proxychains` pour notre proxy web. Par exemple, essayons d'utiliser `cURL`l'un de nos exercices précédents :


```
proxychains curl http://SERVER_IP:PORT

ProxyChains-3.1 (http://proxychains.sf.net)
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <title>Ping IP</title>
    <link rel="stylesheet" href="./style.css">
</head>
...SNIP...
</html>
```


https://github.com/haad/proxychains

