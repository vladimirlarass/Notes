Le protocole `Simple Mail Transfer Protocol`( `SMTP`) permet d'envoyer des e-mails sur un réseau IP. Il peut être utilisé entre un client de messagerie et un serveur de courrier sortant, ou entre deux serveurs SMTP
Par défaut, les serveurs SMTP acceptent les demandes de connexion sur le port `25`
Pour interagir avec le serveur SMTP, nous pouvons utiliser l' `telnet`outil pour initialiser une connexion TCP. L'initialisation de la session s'effectue avec la commande mentionnée ci-dessus, `HELO`ou `EHLO`.\

```shell-session
telnet IP_ADRESS 25
```

```shell-session
sudo nmap 10.129.14.128 -p25 --script smtp-open-relay -v
```

```
smtp-user-enum
```


```
smtp-user-enum -M VRFY -U /usr/share/seclists/Discovery/SNMP/snmp.txt -t 10.129.1.62 
```

| **Commande** | **Description**                                                                                             |
| ------------ | ----------------------------------------------------------------------------------------------------------- |
| `AUTH PLAIN` | AUTH est une extension de service utilisée pour authentifier le client.                                     |
| `HELO`       | Le client se connecte avec son nom d'ordinateur et démarre ainsi la session.                                |
| `MAIL FROM`  | Le client nomme l'expéditeur de l'e-mail.                                                                   |
| `RCPT TO`    | Le client nomme le destinataire de l'e-mail.                                                                |
| `DATA`       | Le client initie la transmission de l'email.                                                                |
| `RSET`       | Le client interrompt la transmission initiée mais maintient la connexion entre le client et le serveur.     |
| `VRFY`       | Le client vérifie si une boîte aux lettres est disponible pour le transfert de messages.                    |
| `EXPN`       | Le client vérifie également si une boîte aux lettres est disponible pour la messagerie avec cette commande. |
| `NOOP`       | Le client demande une réponse du serveur pour éviter une déconnexion due à un dépassement de délai.         |
| `QUIT`       | Le client termine la session.                                                                               |

msf6 auxiliary(scanner/smtp/smtp_enum) 


La liste de tous les codes de réponse SMTP est disponible : 
https://serversmtp.com/smtp-error/