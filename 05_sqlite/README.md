# SQLite

Le système de gestion de base données (SGBD) utilisé pour ce cours est
[SQLite][]. À l'opposé des autres SGBD, un serveur n'est pas nécessaire
pour utiliser SQLite. Les bases de données sont stockés directement sur
le disque de votre ordinateur, et donc facilement accessibles.

[SQLite]: https://sqlite.org/index.html

## Installation

Il est suggérer d'installer SQLite à travers le gestionnaire de paquet
de votre ordinateur : [Homebrew][] sur Mac, et [Scoop][] sur Windows.

[Homebrew]: https://brew.sh
[Scoop]: https://scoop.sh

## Utilisation

Une fois SQLite installé, l'interface en ligne de commande interactive
peut être démarrée avec la commande `sqlite3`. Vous trouverez la
documentation du programme [ici][documentation sqlite3]. Le manuel (`man
sqlite3`) est également une bonne ressource.

[documentation sqlite3]: https://sqlite.org/cli.html

### Mode transitoire

Exécuter la commande `sqlite3` sans aucun argument lance SQLite en mode
transitoire. Le SGBD crée une base de donnée temporaire qui sera perdue
lors de la fermeture du programme. Ce mode est idéal pour pratiquer le
langage SQL, ou pour tester des requêtes.

### Fichier SQL

L'option `-init` suivie du chemin d'un fichier texte permet de démarrer
SQLite et d'exécuter les requêtes SQL qui se trouve dans celui-ci.

