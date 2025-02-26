## C'est quoi l'injection SQL 
L'injection SQL (SQLi) est une vulnérabilité de sécurité Web qui permet à un attaquant d'interférer avec les requêtes qu'une application envoie à sa base de données. Cela peut permettre à un attaquant d'afficher des données qu'il n'est normalement pas en mesure de récupérer.

## Comment détecter les vulnérabilités d'injection SQL

Vous pouvez détecter manuellement une injection SQL à l'aide d'un ensemble systématique de tests sur chaque point d'entrée de l'application. Pour ce faire, vous devez généralement soumettre :

- Le caractère guillemet simple `'`et rechercher des erreurs ou d'autres anomalies.
- Une syntaxe spécifique à SQL qui évalue la valeur de base (d'origine) du point d'entrée et une valeur différente, et recherche des différences systématiques dans les réponses de l'application.
- Conditions booléennes telles que `OR 1=1`et `OR 1=2`, et recherchez les différences dans les réponses de l'application.
- [Récupération de données cachées](https://portswigger.net/web-security/sql-injection#retrieving-hidden-data) , où vous pouvez modifier une requête SQL pour renvoyer des résultats supplémentaires.
- [Subvertir la logique de l'application](https://portswigger.net/web-security/sql-injection#subverting-application-logic) , où vous pouvez modifier une requête pour interférer avec la logique de l'application.
- [Attaques UNION](https://portswigger.net/web-security/sql-injection/union-attacks) , où vous pouvez récupérer des données à partir de différentes tables de base de données.
- [Injection SQL aveugle](https://portswigger.net/web-security/sql-injection/blind) , où les résultats d'une requête que vous contrôlez ne sont pas renvoyés dans les réponses de l'application.






https://portswigger.net/web-security/sql-injection