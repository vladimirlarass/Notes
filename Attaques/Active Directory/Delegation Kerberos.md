
## Qu’est-ce que la délégation Kerberos ?

La délégation Kerberos est un paramètre de délégation qui permet aux applications de demander des informations d’identification d’accès à l’utilisateur final pour accéder aux ressources pour le compte de l’utilisateur d’origine.

Les trois principaux types de délégation que je vais couvrir sont les suivants:
- **Délégation sans contrainte** : tout service peut être utilisé de manière abusive si l’une de ses entrées de délégation est sensible.
- **Délégation contrainte** : les entités contraintes peuvent être abusées si l’une de leurs entrées de délégation est sensible.
- **Délégation contrainte basée sur les ressources (RBCD)** : les entités contraintes basées sur les ressources peuvent être abusées si l’entité elle-même est sensible.

##  Délégation contrainte
La délégation contrainte permet aux administrateurs de configurer les services auxquels un compte utilisateur ou ordinateur peut déléguer et quels protocoles d'authentification peuvent être utilisés