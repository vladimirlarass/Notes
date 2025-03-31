
### GetUSerSPN
permet d'obtenir le hachage du mot de passe des comptes utilisateurs disposant d'un SPN (nom principal de service). Si un SPN est défini sur un compte utilisateur, il est possible de demander un ticket de service pour ce compte et de tenter de le déchiffrer afin de récupérer le mot de passe utilisateur.

GetUserSPNs.py <Domain/Username:password> -dc-ip <ip of DC> -request
https://tools.thehacker.recipes/impacket/examples/getuserspns.py

/home/kali/Documents/HackTheBox/Tools/impacket/examples/GetUserSPNs.py 