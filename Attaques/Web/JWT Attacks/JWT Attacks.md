# Que sont les JWT
les jetons Web JSON (JWT) sont un format standardisé pour l'envoi de données JSON signées de manière cryptographique entre systèmes. Ils peuvent théoriquement contenir n'importe quel type de données, mais sont le plus souvent utilisés pour envoyer des informations (« revendications ») sur les utilisateurs dans le cadre des mécanismes d'authentification, de gestion des sessions et de contrôle d'accès.
### Format JWT

Un JWT se compose de 3 parties : un en-tête , une charge utile et une signature .
 tools 
 JWT Editor https://portswigger.net/bappstore/26aaa5ded2f74beea19e2ed8345a93dd
## les erreurs de validation de signature 
### 1. ne pas verifier la signature
Le premier problème avec la validation de signature survient en l'absence de validation. Si le serveur ne vérifie pas la signature du JWT, il est possible de modifier les revendications du JWT selon vos préférences.

```
curl -H 'Content-Type: application/json' -X POST -d '{ "username" : "user", "password" : "password2" }' http://10.10.34.226/api/v1.0/example2
```

```
curl -H 'Authorization: Bearer [JWT Token]' http://10.10.34.226/api/v1.0/example2?username=user
```

### 2.Retrogradation a aucun 

Un autre problème courant est la dégradation de l'algorithme de signature. Les JWT prennent en charge l' `None`algorithme de signature, ce qui signifie qu'aucune signature n'est utilisée avec le JWT. Même si cela peut paraître absurde, l'idée derrière cette norme était de permettre une communication de serveur à serveur, où la signature du JWT était vérifiée en amont.

https://blog.pentesteracademy.com/hacking-jwt-tokens-the-none-algorithm-67c14bb15771

