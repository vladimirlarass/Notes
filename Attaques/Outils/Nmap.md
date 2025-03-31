`nmap -sV -sC $IP -v -p- --min-rate 1000 -Pn -T4`

II existe au total 6 états différents pour un port scanné que nous pouvons obtenir : 
Open 
Closed 
Filtered
unfiltered
open | filtered
Closed | filtered 
`-Pn`   Désactive les requêtes ICMP Echo.
'` nmap 10.129.2.0/24 -F | grep "/tcp" | wc -l`'

## Leurres

Il existe des cas dans lesquels les administrateurs bloquent en principe des sous-réseaux spécifiques de différentes régions. Cela empêche tout accès au réseau cible. Un autre exemple est lorsque IPS doit nous bloquer. Pour cette raison, la méthode de balayage Decoy ( `-D`) est le bon choix. Avec cette méthode, Nmap génère diverses adresses IP aléatoires insérées dans l'en-tête IP pour masquer l'origine du paquet envoyé. Avec cette méthode, nous pouvons générer aléatoirement ( `RND`) un nombre spécifique (par exemple : `5`) d'adresses IP séparées par deux points ( `:`). Notre véritable adresse IP est ensuite placée aléatoirement entre les adresses IP générées. Dans l'exemple suivant, notre véritable adresse IP est donc placée en deuxième position. Un autre point critique est que les leurres doivent être vivants. Sinon, le service sur la cible peut être inaccessible en raison des mécanismes de sécurité SYN-flooding.

```
nmap $ip  -p 80 -sS -Pn -n --disable-arp-ping --packet-trace -D RND:5
```

|   |   |
|---|---|
|`-p 80`|Analyse uniquement les ports spécifiés.|
|`-sS`|Exécute une analyse SYN sur les ports spécifiés.|
|`-Pn`|Désactive les requêtes ICMP Echo.|
|`-n`|Désactive la résolution DNS.|
|`--disable-arp-ping`|Désactive le ping ARP.|
|`--packet-trace`|Affiche tous les paquets envoyés et reçus.|
|`-D RND:5`|Génère cinq adresses IP aléatoires qui indiquent l'IP source d'où provient la connexion.|

```
`nmap 10.129.96.164 -n -Pn -p 80 -O -S 10.10.15.130 -e tun0 -sV`
```

# Proxy DNS

Par défaut, `Nmap`effectue une résolution DNS inverse, sauf indication contraire, pour trouver des informations plus importantes sur notre cible. Ces requêtes DNS sont également transmises dans la plupart des cas, car le serveur Web donné est censé être trouvé et visité. Les requêtes DNS sont effectuées via le `UDP port 53`. Le `TCP port 53`était auparavant utilisé uniquement pour les " `Zone transfers`" entre les serveurs DNS ou le transfert de données supérieur à 512 octets. De plus en plus, cela change en raison des extensions IPv6 et DNSSEC. Ces changements entraînent de nombreuses requêtes DNS via le port TCP 53.

Cependant, cela `Nmap`nous donne toujours un moyen de spécifier nous-mêmes les serveurs DNS ( `--dns-server <ns>,<ns>`). Cette méthode pourrait nous être fondamentale si nous nous trouvons dans une zone démilitarisée ( `DMZ`). Les serveurs DNS de l'entreprise sont généralement plus fiables que ceux d'Internet. Ainsi, par exemple, nous pourrions les utiliser pour interagir avec les hôtes du réseau interne. Comme autre exemple, nous pouvons utiliser `TCP port 53`comme port source ( `--source-port`) pour nos analyses. Si l'administrateur utilise le pare-feu pour contrôler ce port et ne filtre pas correctement les IDS/IPS, nos paquets TCP seront approuvés et transmis.


```
nmap 10.129.2.28 -p50000 -sS -Pn -n --disable-arp-ping --packet-trace --source-port 53
```



```
ncat -nv --source-port 53 10.129.150.182 50000
```
