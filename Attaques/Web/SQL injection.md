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

Type d'injection SQL
In-Band : 
	union Based : nous pouvons être amenés à spécifier l'emplacement exact (c'est-à-dire la colonne) que nous pouvons lire, afin que la requête y affiche le résultat.
	et Error Based : est utilisée lorsque nous pouvons obtenir les erreurs `PHP`« ou » `SQL`dans le front-end, et ainsi provoquer intentionnellement une erreur SQL renvoyant le résultat de notre requête.
Blind  :les cas plus complexes, il est possible que la sortie ne soit pas imprimée ; nous pouvons alors utiliser la logique SQL pour récupérer la sortie caractère par caractère
	Boolean Based :nous pouvons utiliser des instructions conditionnelles SQL pour contrôler si la page renvoie une sortie, c'est-à-dire la réponse à la requête d'origine, si notre instruction conditionnelle renvoie `true`.
	Time based :nous utilisons des instructions conditionnelles SQL qui retardent la réponse de la page si l'instruction conditionnelle renvoie `true`via la `Sleep()`fonction.
Out-Of-Band : dans certains cas, nous n'avons pas d'accès direct à la sortie ; nous devrons alors la diriger vers un emplacement distant, par exemple un enregistrement DNS, puis tenter de la récupérer à partir de là.


## Contournement de l'authentification
## Découverte SQLi

Avant de contourner la logique de l'application web et de tenter de contourner l'authentification, nous devons d'abord vérifier si le formulaire de connexion est vulnérable à une injection SQL. Pour ce faire, nous allons essayer d'ajouter l'une des charges utiles suivantes après notre nom d'utilisateur et voir si cela provoque des erreurs ou modifie le comportement de la page :
## OU Injection 
Pour contourner l'authentification, la requête doit toujours renvoyer `true`, quels que soient le nom d'utilisateur et le mot de passe saisis. Pour ce faire, nous pouvons utiliser l' `OR`opérateur dans notre injection SQL.
Un exemple de condition qui renvoie toujours `true`la valeur `'1'='1'`. Cependant, pour que la requête SQL fonctionne et conserve un nombre pair de guillemets, au lieu d'utiliser ('1'='1'), nous supprimerons le dernier guillemet et utiliserons ('1'='1), de sorte que le guillemet simple restant de la requête d'origine soit à sa place.

Donc, si nous injectons la condition ci-dessous et avons un `OR`opérateur entre elle et la condition d'origine, elle devrait toujours renvoyer `true`:







https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/SQL%20Injection#authentication-bypass
https://portswigger.net/web-security/sql-injection
- [PortSwigger - SQL injection vulnerability in WHERE clause allowing retrieval of hidden data](https://portswigger.net/web-security/sql-injection/lab-retrieve-hidden-data)
- [PortSwigger - SQL injection vulnerability allowing login bypass](https://portswigger.net/web-security/sql-injection/lab-login-bypass)
- [PortSwigger - SQL injection with filter bypass via XML encoding](https://portswigger.net/web-security/sql-injection/lab-sql-injection-with-filter-bypass-via-xml-encoding)
- [PortSwigger - SQL Labs](https://portswigger.net/web-security/all-labs#sql-injection)
- [Root Me - SQL injection - Authentication](https://www.root-me.org/en/Challenges/Web-Server/SQL-injection-authentication)
- [Root Me - SQL injection - Authentication - GBK](https://www.root-me.org/en/Challenges/Web-Server/SQL-injection-authentication-GBK)
- [Root Me - SQL injection - String](https://www.root-me.org/en/Challenges/Web-Server/SQL-injection-String)
- [Root Me - SQL injection - Numeric](https://www.root-me.org/en/Challenges/Web-Server/SQL-injection-Numeric)
- [Root Me - SQL injection - Routed](https://www.root-me.org/en/Challenges/Web-Server/SQL-Injection-Routed)
- [Root Me - SQL injection - Error](https://www.root-me.org/en/Challenges/Web-Server/SQL-injection-Error)
- [Root Me - SQL injection - Insert](https://www.root-me.org/en/Challenges/Web-Server/SQL-injection-Insert)
- [Root Me - SQL injection - File reading](https://www.root-me.org/en/Challenges/Web-Server/SQL-injection-File-reading)
- [Root Me - SQL injection - Time based](https://www.root-me.org/en/Challenges/Web-Server/SQL-injection-Time-based)
- [Root Me - SQL injection - Blind](https://www.root-me.org/en/Challenges/Web-Server/SQL-injection-Blind)
- [Root Me - SQL injection - Second Order](https://www.root-me.org/en/Challenges/Web-Server/SQL-Injection-Second-Order)
- [Root Me - SQL injection - Filter bypass](https://www.root-me.org/en/Challenges/Web-Server/SQL-injection-Filter-bypass)
- [Root Me - SQL Truncation](https://www.root-me.org/en/Challenges/Web-Server/SQL-Truncation)