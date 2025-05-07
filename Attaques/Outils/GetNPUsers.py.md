Peut être utilisé pour récupérer les utilisateurs du domaine qui ont défini « Ne pas exiger de pré-authentification Kerberos » et demander leurs TGT sans connaître leurs mots de passe. Il est alors possible de tenter de déchiffrer la clé de session envoyée avec le ticket pour récupérer le mot de passe de l'utilisateur. 

our vérifier si l'un de ces noms d'utilisateur est valide, nous utiliserons le script GetNPUsers.py. 

```
./GetNPUsers.py Domain_name/ -usersfile Usernames.txt -dc-ip 10.129.28.119
```




https://tools.thehacker.recipes/impacket/examples/getnpusers.py#commons
