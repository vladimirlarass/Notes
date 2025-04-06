OAuth 2.0 propose plusieurs types d'autorisations pour s'adapter à différents scénarios et types de clients
## Octroi de code d'autorisation
Ce type d'autorisation est reconnu pour sa sécurité renforcée, car le code d'autorisation est échangé contre un jeton d'accès de serveur à serveur. Ce jeton n'est donc pas exposé à l'agent utilisateur (par exemple, le navigateur), réduisant ainsi le risque de fuite de jeton. Il prend également en charge l'utilisation de jetons d'actualisation pour maintenir un accès à long terme sans authentification répétée de l'utilisateur.

## Subvention implicite
L'autorisation implicite est principalement conçue pour les applications mobiles et web où les clients ne peuvent pas stocker leurs secrets de manière sécurisée. Elle **émet directement le jeton d'accès au client sans nécessiter l'échange d'un code d'autorisation** . Dans ce flux, le client redirige l'utilisateur vers le serveur d'autorisation. Une fois l'utilisateur authentifié et l'autorisation accordée, le serveur d'autorisation renvoie un jeton d'accès **dans le fragment d'URL**

## Mot de passe du propriétaire de la ressource

L'attribution d'identifiants de mot de passe du propriétaire de la ressource est utilisée lorsque le client bénéficie **d'une confiance élevée de la part du propriétaire de la ressource** , comme dans le cas d'applications propriétaires. Le client collecte directement les identifiants de l'utilisateur (nom d'utilisateur et mot de passe) et les échange contre un jeton d'accès,

## Octroi d'accréditations client

L'attribution des identifiants client est utilisée pour les interactions de serveur à serveur sans intervention de l'utilisateur. Le client utilise ses identifiants pour s'authentifier auprès du serveur d'autorisation et obtenir un jeton d'accès.

