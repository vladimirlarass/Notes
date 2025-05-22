[_Sliver_](https://github.com/BishopFox/sliver) est un framework de commande et de contrôle (C2) open source et multiplateforme publié en 2020 par [_BishopFox_](https://bishopfox.com/) _._ Développé pour offrir une alternative robuste aux outils de tests d'intrusion traditionnels, Sliver C2 offre une suite unique de fonctionnalités permettant une interaction dynamique avec les systèmes compromis. Conçu pour échapper à la détection.
un framework open source multiplateforme d'émulation d'adversaires et de red team.

## Installation 

L'architecture de Sliver est client-serveur. Plusieurs clients, appelés « operators » ou « Players », se connectent au même serveur et collaborent sur une mission. Il faut donc configurer le serveur, puis distribuer l'application cliente et ses identifiants à tous les opérateurs. Pour simplifier les choses, j'utiliserai une seule machine virtuelle Kali. 

Nous pouvons faire une installation de base à partir de binaires compilés, puis montrer comment compiler à partir des sources. Je recommande de compiler à partir des sources, car cela permet de modifier rapidement les éléments si nécessaire.
###  Installation de base à partir des binaires de publication

Sliver fournit un [script d'installation](https://github.com/BishopFox/sliver/blob/master/docs/install) qui vous permet d'être opérationnel en un rien de temps. Vous pouvez l'exécuter avec `curl https://sliver.sh/install|sudo bash`. Si vous ne souhaitez pas envoyer des scripts distants à l'aveugle vers un shell root aujourd'hui, lisez ce qui suit. Cette installation a été effectuée manuellement sur Kali Linux, mais devrait fonctionner globalement de la même manière sur de nombreuses autres distributions Linux.

### Construire à partir de la source

Créer un fragment à partir des sources est simple. Il suffit de suivre les étapes décrites dans [leur wiki](https://github.com/BishopFox/sliver/wiki/Compile-From-Source) . Par souci d'exhaustivité, voici une description du processus au moment de la rédaction.

Pour compiler à partir des sources, vous devez installer Go. Pour cela, utilisez `apt-get install golang`. Récupérez ensuite les sources sur GitHub. Clonez le dépôt n'importe où sur votre machine avec `git clone https://github.com/BishopFox/sliver` et accédez à son dossier racine.

Vous devez maintenant télécharger toutes les ressources à l'aide du `go-assets.sh`script. Le téléchargement de toutes les ressources Go statiques génère beaucoup de résultats.

https://github.com/BishopFox/sliver
https://sliver.sh/docs?name=Getting+Started
https://dominicbreuker.com/post/learning_sliver_c2_01_installation/