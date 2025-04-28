Kerberos est un protocole d'authentification réseau. Il est conçu pour fournir une authentification forte pour les applications client/serveur en utilisant la cryptographie à clé secrète.
Kerberos est actuellement (au moment de la rédaction de cet article) à la version 1.21.2 de la version 5. La version 5 a été publiée pour la première fois en 1993, en tant que norme proposée dans [la RFC 4120](https://datatracker.ietf.org/doc/html/rfc4120#section-1.1) .
##  l'authentification Kerberos 
Le nom du service d’authentification Kerberos a été inspiré par Cerbère de la mythologie grecque : un gigantesque chien à trois têtes qui gardait les portes des enfers (alias le « chien d’Hadès »).  

Un autre trait particulier (et peut-être le plus utile) de Cerbère est qu'il « refusait l'entrée aux humains vivants ». ( _Non sans une authentification et une autorisation appropriées, bien sûr, toujours en suivant les politiques strictes d'IAM d'Hadès. Même en enfer, il faut suivre le principe du moindre privilège.)

## **Les trois têtes de l'authentification Kerberos : KDC, domaines et tickets**


le protocole Kerberos par ces trois composants principaux :

1. **Principal :** le principal est l'identité qui souhaite s'authentifier. Selon la terminologie Kerberos du MIT lui-même, « il s'agit de l'identité que vous utilisez pour vous connecter à Kerberos ». N'oubliez pas que cela peut faire référence à un principal d'utilisateur ou à un principal de service.
    
    **1A :** Un utilisateur principal serait un utilisateur à authentifier, tel que « Orpheus@UNDERWORLD.CORP ». Dans cet exemple, Orpheus est l'utilisateur avec le principal « Orpheus » dans le domaine « UNDERWORLD.CORP ».
    
    **1B :** Un principal de service représente un service commun, tel qu'un serveur HTTP ou un partage de fichiers. Considérez « HTTP/hadesshares.underworld.internal@UNDERWORLD.CORP » comme un principal de service ; ce principal de service spécifique se rapporte à un serveur HTTP fonctionnant dans le domaine « UNDERWORLD.CORP ». Dans ce principal de service, « HTTP » sert d'identifiant pour le service HTTP, « hadesshares.underworld.com » signifie le nom de domaine complet (FQDN) du serveur Web et « UNDERWORLD.CORP » désigne le domaine dans lequel ce serveur HTTP particulier est situé.
    
2. **Ressource :** dans le contexte de Kerberos, une ressource fait référence à l'actif ou au service réseau qu'un client souhaite atteindre. Cela peut englober les services réseau, les systèmes ou les données avec lesquels un utilisateur ou un service cherche à interagir après avoir terminé avec succès le processus d'authentification.
    
3. **Centre de distribution de clés (KDC)** : Le centre de distribution de clés. Un composant central de l'authentification Kerberos. Le KDC est responsable de la gestion de l'authentification et de la distribution des clés de session dans un royaume.
### Le centre de distribution de clés (KDC)


le centre de distribution de clés (KDC) dispose également d'autres composants de sécurité. Nous pouvons les séparer en plusieurs parties :

1. **Base de données Kerberos** . Elle stocke les informations d'authentification nécessaires concernant le principal et les systèmes et services auxquels il peut s'authentifier.
    
2. **Serveur d'authentification Kerberos (AS)** . Les principaux utilisent ce service Kerberos pour s'authentifier afin d'obtenir un **ticket d'octroi de ticket (TGT)** , également appelé ticket d'authentification (plus d'informations sur les tickets à venir).
    
3. **Service d'octroi de tickets Kerberos (TGS)** . Ce service Kerberos accepte le TGT afin que les clients puissent accéder à leurs serveurs d'applications.

### Billets Kerberos

Le concept de tickets est crucial pour Kerberos. C'est là que la magie opère. Il s'agit de la fonctionnalité principale de Kerberos qui empêche la transmission des mots de passe en texte clair et permet une connexion unique pour accéder à plusieurs services et hôtes.

Les tickets utilisent un chiffrement asymétrique. Ils contiennent deux clés de chiffrement :

1. **La clé du ticket** : Partagée entre l'infrastructure Kerberos et le service demandé par le principal.
    
2. **La clé de session** : partagée entre le principal et le service demandé. Utilisée pour chiffrer et déchiffrer la communication avec le service. (Il faut faire attention à ces attaques Typhon In The Middle, vous vous souvenez ? _Nous savons tous maintenant que nous faisons référence à MITM ou attaque sur le chemin, n'est-ce pas ?_ )
### Explication du processus d'authentification du réseau Kerberos

Décomposons comment ces composants fonctionnent ensemble en considérant le scénario suivant :

> Orphée veut se connecter au serveur de fichiers d'Hadès pour vérifier des informations secrètes sur la localisation d'Eurydice. Si le royaume des morts d'Hadès était un royaume Kerberos, voici le processus d'authentification du client :

## **1. Qui est là ?**

1. Orphée doit d’abord s’authentifier sur le serveur d’authentification (AS) et non sur le serveur d’applications auquel il tente d’accéder.
    
2. AS valide les informations d'identification reçues. Il recherche le hachage du mot de passe de l'utilisateur « Orpheus » et décrypte l'horodatage.
    
3. Si le déchiffrement est réussi et l’horodatage unique, l’AS authentifie Orpheus.
    
4. Orpheus reçoit une réponse du serveur d'authentification (AS-REP) de l'AS. Livré avec :
- Clé de session, cryptée avec le hachage Orphée (afin qu'Orphée puisse la décrypter).
    
- Billet d'octroi de billet (TGT).
    

## **2. Je vous accorde la permission !**

Après authentification et équipé d'un TGT, Orphée peut maintenant demander l'autorisation d'accéder au serveur de fichiers. Devinez quoi ? Il doit recontacter le KDC. Le serveur d'octroi de tickets (TGS) du KDC entre alors en jeu :
1. Orpheus envoie une demande de serveur d'octroi de tickets (TGS-REQ) contenant :

- Nom d'utilisateur et horodatage chiffrés avec la clé de session.
    
- Nom de la ressource à laquelle il tente d'accéder (serveur de fichiers contenant des informations secrètes sur la localisation d'Eurydice).
    
- Le TGT crypté qu'il a reçu lors de l'authentification.
2.  Dès réception du TGS-REQ, le service d'octroi de billets du KDC :

- Vérifiez si la ressource existe dans le royaume.
    
- Décrypter le TGT.
    
- Extraire les clés de session du TGT.
    

3. Si tout est correct, le TGS répondra à Orpheus avec une réponse du serveur d'octroi de tickets (TGS-REP), fournie avec :

## **3. Orphée a reçu un billet pour voyager**

Maintenant qu'Orpheus est authentifié par le KDC, et dispose d'une clé de session et d'un ticket de service (ST), il peut désormais communiquer avec le service (enfin !) :
-   
    Orpheus envoie au serveur d'applications une demande d'application (AP-REQ), accompagnée de :
    
    Ticket de service (TS).
    
    Nom d'utilisateur et horodatage chiffrés avec la clé de session.
    
- Le ticket de service (ST) est déchiffré avec la clé secrète du serveur d'applications (partagée avec le TGS).
    
- Le serveur d'applications vérifie si le nom d'utilisateur AP-REQ correspond à celui déchiffré du ST.
    
- Vérifie ensuite si le principal (Orpheus) dispose des privilèges nécessaires pour utiliser le service.
    
- Si tout est correct, le serveur d'applications envoie une réponse de serveur d'applications AP-REP accordant l'accès à la ressource demandée.
    
- Orphée peut désormais s'authentifier auprès du partage de fichiers et établir une session sécurisée à l'aide de la clé de session incluse dans l'AP-REP.
    
- Va chercher Eurydice Orphée. Mais ne te retourne pas.


https://www.hackthebox.com/blog/what-is-kerberos-authentication