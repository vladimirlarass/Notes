Rubeus est un outil puissant pour attaquer Kerberos. Adapté de l'outil Kekeo,
Rubeus propose une grande variété d'attaques et de fonctionnalités qui en font un outil très polyvalent pour attaquer Kerberos. Parmi ses nombreux outils et attaques, on peut citer le dépassement de hachage, les demandes et renouvellements de tickets, la gestion et l'extraction de tickets, la collecte, la transmission de tickets, l'AS-REP Roasting et le Kerberoasting.
[https://github.com/GhostPack/Rubeus](https://github.com/GhostPack/Rubeus)

## Récolte de Tickets 

```
Rubeus.exe harvest /interval:30
```

Cette commande indique a Rubeux de recolter des TGT toutes les 30 secondes

## Force Brute / Pulverisation de mot de passe 

Cette attaque utilise un mot de passe Kerberos donné, le diffuse sur tous les utilisateurs détectés et génère un ticket .kirbi. Ce ticket est un TGT permettant d'obtenir des tickets de service auprès du KDC, ainsi que dans des attaques telles que l'attaque par transfert de ticket.

```
Rubeus.exe brute /password:Password1 /noticket
```

Cela prendra un mot de passe donné et le « vaporisera » sur tous les utilisateurs trouvés, puis donnera le TGT .kirbi pour cet utilisateur