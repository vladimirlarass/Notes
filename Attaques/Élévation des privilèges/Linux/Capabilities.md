
Pour effectuer des contrôles d'autorisation, UNIX traditionnelles distinguent deux catégories de processus :
_processus privilégiés_ (dont l'ID utilisateur effectif est 0, appelé en tant que superutilisateur ou root) et les processus _non privilégiés_ (dont UID effectif est différent de zéro). Les processus privilégiés contournent tous vérifications des autorisations du noyau, tandis que les processus non privilégiés sont sous réserve d'une vérification complète des autorisations en fonction du processus informations d'identification (généralement : UID effectif, GID effectif et liste des groupes supplémentaires).
À partir de Linux 2.2, Linux divise les privilèges traditionnellement associé au superutilisateur en unités distinctes,connues sous le nom _de capabilities_ , qui peuvent être activées indépendamment et désactivé. Les capabilities sont un attribut par thread.
### Lister les capacités des binaires



```
~$ find / -perm -u=s -type f 2>/dev/null
```









https://man7.org/linux/man-pages/man7/capabilities.7.html
[https://gtfobins.github.io/](https://gtfobins.github.io/)
