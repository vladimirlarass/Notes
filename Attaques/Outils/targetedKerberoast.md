
targetedKerberoast est un script Python qui, comme beaucoup d'autres (par exemple, [GetUserSPNs.py](https://github.com/SecureAuthCorp/impacket/blob/master/examples/GetUserSPNs.py) ), peut afficher les hachages « kerberoast » des comptes utilisateurs disposant d'un SPN défini.

Cet outil prend en charge les authentifications suivantes

- (NTLM) Mot de passe en texte clair
- (NTLM) Pass-the-hash
- (Kerberos) Mot de passe en clair
- (Kerberos) Passer la clé / Dépasser le hachage
- (Kerberos) Pass-the-cache (ype de Pass-the-ticket
- 
```bash
targetedKerberoast.py -v -d 'domain.local' -u 'controlledUser' -p 'ItsPassword'
```