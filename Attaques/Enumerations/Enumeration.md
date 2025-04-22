Etape 1 : La presence sur internet 
La première couche à franchir est la couche « Présence Internet », où nous nous concentrons sur la recherche des cibles à examiner. 
https://crt.sh/

```shell-session
curl -s https://crt.sh/\?q\=inlanefreight.com\&output\=json | jq . | grep name | cut -d":" -f2 | grep -v "CN=" | cut -d'"' -f2 | awk '{gsub(/\\n/,"\n");}1;' | sort -u
```

les serveur heberge par l'entreprise 
```
for i in $(cat subdomainlist);do host $i | grep "has address" | grep inlanefreight.com | cut -d" " -f1,4;done
```

https://domain.glass/
https://buckets.grayhatwarfare.com/

Etape 2 : La paserelle 
Etape 3 : Service Accessible 
Etape 4 : les processus 
Etape 5 : Privilege 
Etape 6 : Configuration du systeme d'exploitation 
