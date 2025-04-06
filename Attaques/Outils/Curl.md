recuperer le HEAD 

```
 curl -i http://exemple
```

recuperer la version du TLS 

```
curl -v https://10.10.10.7 2>&1 | grep "SSL connection using"
```

Affiche la page sans information inutile 
```
curl -s http://exemple.htm
```