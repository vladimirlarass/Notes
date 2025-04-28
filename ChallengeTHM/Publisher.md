# Enumeration 
## Nmap 
Nous commençons par exécuter une analyse nmap sur la cible :

![[Pasted image 20250422222347.png]]

le scan nous montre que le port 22 et 80 sont ouverts ,

# Webserver 
je tombe sur un site web anodin 

![[Pasted image 20250422224600.png]]
apres enumeration des repertoire je tombe sur /spip


![[Pasted image 20250422223944.png]]

![[Pasted image 20250422225019.png]]

https://github.com/rapid7/metasploit-framework/pull/17711/files

Apres plusieurs heure j'ai essaaye la le framework metasploit 

![[Pasted image 20250423001854.png]]

![[Pasted image 20250423001929.png]]

# PATH Injection 
App Armor est un outil qui permet de restreindre un outil avec des règles. Vous pouvez accéder à ces informations par une simple recherche sur Google. j'ai eu ceette indication sur la salle 
je suis alle regarder les fichiers de apparmor  pour mieux comprendre les regles 

![[Pasted image 20250423142217.png]]

j'ajoute le dossier /var/tmp donc j'ai les droits d'ecriture 

Je crée ensuite un fichier binaire appelé docker et l'autorise à s'exécuter. Ce fichier contient « /bin/bash -p ». Je souhaite donc utiliser la commande bash en mode privilégié.
Si vous faites attention, lorsque j'imprime la valeur $PATH, le shell que nous voyons au début est ash, mais après avoir effectué l'opération, il devient bash.

Ensuite, je copie mon fichier docker dans /opt/run_container.sh et j'exécute mon binaire **/usr/sbin/run_container** avec le bit SUID et je suis ROOT !!!

![[Pasted image 20250423143022.png]]

![[Pasted image 20250423143138.png]]


