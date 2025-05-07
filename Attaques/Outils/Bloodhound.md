La reconnaissance est une étape clé dans toute campagne de Red Team ou d’audit de sécurité Active Directory. **BloodHound** est un outil puissant permettant de visualiser et d’analyser les relations d’un domaine Active Directory en utilisant la théorie des graphes.

## BloodHound : Présentation

BloodHound est une application qui cartographie les relations entre les objets Active Directory (utilisateurs, groupes, ordinateurs, sessions, délégations, ACL, etc.) pour identifier des chemins d’attaque possibles, tels que les escalades de privilèges ou les mouvements latéraux.

Il repose sur une base de données orientée graphe, **Neo4j**, et une interface graphique puissante qui permet d'exécuter des requêtes en langage **Cypher** pour analyser les chemins d'attaque.

## Collecte de données avec bloodhound-python

L’outil classique `SharpHound` peut être remplacé par [`bloodhound-python`](https://github.com/fox-it/BloodHound.py), une alternative sans agent écrite en Python. Cela permet une collecte depuis une machine Linux ou directement via un shell Python dans certains cas de compromission.

### Exemple de commande complète :

```
sudo bloodhound-python -d <domain> -u <username>@<domain> -p <password> -c all -ns <domain_controller_ip> -v
```

Une fois la collecte terminée, vous obtiendrez un ou plusieurs fichiers `.json.gz` à importer dans l’interface BloodHound.

```
neo4j console
```

http://localhost:7474/
Bloodhound


https://bloodhound.readthedocs.io/en/latest/index.html
https://hausec.com/2019/09/09/bloodhound-cypher-cheatsheet/