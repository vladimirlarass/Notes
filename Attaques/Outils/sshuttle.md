# sshuttle : là où le proxy transparent rencontre le VPN et SSH


Autant que je sache, sshuttle est le seul programme qui résout le cas courant suivant :

- Votre machine cliente (ou routeur) est Linux, FreeBSD, MacOS ou Windows.
- Vous avez accès à un réseau distant via ssh.
- Vous n’avez pas nécessairement accès administrateur sur le réseau distant.
- Le réseau distant n'a pas de VPN, ou seulement des protocoles VPN complexes (IPsec, PPTP, etc.). Ou peut-être _êtes-_ vous l'administrateur et êtes-vous simplement frustré par l'état lamentable des outils VPN ?
- Vous ne souhaitez pas créer une redirection de port SSH pour chaque hôte/port du réseau distant.
- Vous détestez la redirection de port d'OpenSH parce qu'elle est aléatoirement lente et/ou stupide.
- Vous ne pouvez pas utiliser la fonctionnalité PermitTunnel d'OpenSH car elle est désactivée par défaut sur les serveurs OpenSH ; de plus, elle utilise TCP-over-TCP, ce qui a des performances terribles .