### **1. Comptes Utilisateurs Intégrés (Builtin Users)**

- **Administrator** : Compte administrateur principal du domaine avec tous les droits. Il ne peut pas être supprimé mais peut être renommé.
- **Guest** : Compte invité, souvent désactivé par défaut, avec des permissions très limitées. Il peut être renommé ou supprimé.
- **krbtgt** : Compte utilisé pour le service Kerberos, responsable de l’émission des tickets d’authentification. Il est désactivé par défaut et son mot de passe doit être changé périodiquement.
- **DefaultAccount** : Compte système utilisé pour des opérations internes dans Windows. Présent depuis Windows 10 et Windows Server 2016.
- **DWM-1 (Desktop Window Manager)** : Utilisé pour la gestion de l'affichage graphique des sessions utilisateur.
- **System (NT AUTHORITY\SYSTEM)** : Utilisé pour exécuter les services locaux avec les droits les plus élevés sur la machine.

---

### **2. Comptes d’Ordinateurs (Computer Accounts)**

Tous les ordinateurs joints au domaine ont un compte créé automatiquement sous **CN=Computers** ou une autre OU.

- **Nom du compte** = NOM-MACHINE$
- Utilisé pour l’authentification et la communication avec l’Active Directory.

---

### **3. Groupes de Sécurité Intégrés (Built-in Security Groups)**

- **Domain Admins** : Groupe ayant un contrôle total sur le domaine.
- **Enterprise Admins** : Groupe ayant un contrôle total sur l’ensemble de la forêt AD.
- **Schema Admins** : Groupe autorisé à modifier le schéma Active Directory.
- **Administrators** : Groupe administrant localement toutes les machines jointes au domaine.
- **Users** : Contient tous les utilisateurs standards du domaine.
- **Guests** : Contient les utilisateurs invités, avec des permissions limitées.
- **Domain Computers** : Contient tous les comptes d’ordinateurs du domaine.
- **Domain Controllers** : Contient tous les contrôleurs de domaine.
- **Read-Only Domain Controllers (RODC)** : Contient les contrôleurs de domaine en lecture seule.
- **Remote Desktop Users** : Groupe autorisant l’accès RDP aux machines du domaine.
- **DnsAdmins** : Groupe administrant le service DNS du domaine.
- **Backup Operators** : Groupe pouvant sauvegarder et restaurer des fichiers sur le serveur.
https://learn.microsoft.com/fr-fr/windows-server/identity/ad-ds/manage/understand-default-user-accounts