## version 
  
Énumération des versions principales de WordPress
```
curl -s -X GET http://blog.inlanefreight.com | grep '<meta name="generator"'
```

Enumeration des Pluging et des themes 
Pluging 
```
curl -s -X GET http://blog.inlanefreight.com | sed 's/href=/\n/g' | sed 's/src=/\n/g' | grep 'wp-content/plugins/*' | cut -d"'" -f2
```

Theme 

```
curl -s -X GET http://blog.inlanefreight.com | sed 's/href=/\n/g' | sed 's/src=/\n/g' | grep 'themes' | cut -d"'" -f2
```

Enumeration Utilisateur 

```
curl -s -I http://blog.inlanefreight.com/?author=1
```

```
curl http://blog.inlanefreight.com/wp-json/wp/v2/users | jq
```

```
curl -X POST -d "<methodCall><methodName>wp.getUsersBlogs</methodName><params><param><value>admin</value></param><param><value>CORRECT-PASSWORD</value></param></params></methodCall>" http://blog.inlanefreight.com/xmlrpc.php
```