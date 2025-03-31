# C'est quoi un SID

Un identificateur de sécurité est utilisé pour identifier de manière unique un principal de sécurité ou un groupe de sécurité .Les principaux de sécurité représentent toute entité qui peut être authentifiée par le système d’exploitation, comme un compte d’utilisateur, un compte d’ordinateur, ou un thread ou un processus qui s’exécute dans le contexte de sécurité d’un compte d’utilisateur ou d’ordinateur.

Chaque fois qu’un utilisateur se connecte, le système crée un jeton d’accès pour cet utilisateur. Le jeton d’accès contient le SID de l’utilisateur, les droits de l’utilisateur et les SID des groupes auxquels l’utilisateur appartient. Ce jeton fournit le contexte de sécurité pour toutes les actions effectuées par l’utilisateur sur cet ordinateur.

# Architecture de l’identificateur de sécurité

Un identificateur de sécurité est une structure de données au format binaire, qui contient un nombre variable de valeurs. Les premières valeurs de la structure contiennent des informations sur la structure SID.
es composants d’un SID sont plus faciles à visualiser lorsque les SID sont convertis d’un format binaire à un format de chaîne à l’aide de la notation standard :

`S-R-X-Y1-Y2-Yn-1-Yn`

Par exemple, le SID du groupe intégré Administrateurs est représenté en notation SID standardisée avec une chaîne sous la forme suivante :

`S-1-5-32-544`

Ce SID comporte quatre composants :

- Niveau de révision (1)
- Valeur d’autorité d’identificateur (5, Autorité NT)
- Identificateur de domaine (32, Builtin)
- Identificateur relatif (544, Administrateurs

## SID connus

Les valeurs de certains SID sont constantes, quel que soit le système. Ces SID sont créés lorsque le système d’exploitation ou le domaine est installé. Ils sont appelés SID connus, car ils identifient les utilisateurs génériques ou les groupes génériques.


https://learn.microsoft.com/fr-fr/windows-server/identity/ad-ds/manage/understand-security-identifiers