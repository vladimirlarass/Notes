
Active Directory Domain Services (AD DS) assure la sécurité entre plusieurs domaines ou forêts via des relations de confiance de domaine et de forêt.

## Flux de relation de confiance
### Relations de confiance unidirectionnelles et bidirectionnelles

Les relations d’approbation permettent d’accéder aux ressources de manière unidirectionnelle ou bidirectionnelle.

Une approbation unidirectionnelle est un chemin d’authentification unidirectionnel créé entre deux domaines. Dans une approbation unidirectionnelle entre _Domaine A_ et _Domaine B_, les utilisateurs de _Domaine A_ peuvent accéder aux ressources dans _Domaine B_. Toutefois, les utilisateurs de _Domaine B_ ne peuvent pas accéder aux ressources dans _Domaine A_.

Certaines approbations unidirectionnelles peuvent être non transitives ou transitives en fonction du type d’approbation créé.

Dans une approbation bidirectionnelle, _domaine A_ approuve _domaine B_ et _domaine B_ approuve _domaine A_. Cette configuration signifie que les demandes d’authentification peuvent être passées entre les deux domaines dans les deux sens. Certaines relations bidirectionnelles peuvent être non transitives ou transitives en fonction du type de confiance créé.






https://learn.microsoft.com/fr-fr/entra/identity/domain-services/concepts-forest-trust