Lorsque l’authentification se produit dans Kerberos, la première chose qui se produit est une demande d’authentification au contrôleur de domaine afin que l’identité qui tente de s’authentifier puisse être vérifiée.
Cette demande est connue sous le nom de demande de serveur d’authentification (AS-REQ).
Une fois l'authentification du client validée, le contrôleur de domaine envoie une réponse du serveur d'authentification (AS-REP) au client, contenant une clé de session et un ticket d'octroi de ticket (TGT).

https://github.com/fortra/impacket/blob/master/examples/GetNPUsers.py
https://www.hackthebox.com/blog/as-rep-roasting-detection