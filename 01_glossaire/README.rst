Base de données
    Collection organisée de **données**. À ne pas confondre avec
    **système de gestion de base de données**.

Donnée
    Ce qui est connu.

Système de gestion de base de données (SGBD)
    Logiciel conçu pour stocker, maintenir et utiliser une base de
    données. Un SGBD nous permet de définir une base de données en
    terme de son **modèle de données**.

    MySQL, MariaDB, PostgreSQL, SQLite sont des exemples de SGBD qui
    utilisent un **modèle de données relationnel**.

Modèle de données
    Manière de décrire des données sans avoir à se soucier de
    comment ces données sont stockées concrètement. La majorité des
    SGDB sont basés sur le **modèle relationnel**.

    Souvent, une base de données est d'abord conçue à l'aide d'un
    **modèle sémantique de données**, lequel est ensuite traduit en
    **schémas**.


Modèle sémantique de données
    Manière abstraite, souvent graphique, de décrire une base de
    données. Un des modèles sémantiques les plus utilisés est le
    modèle entité-association.

Modèle relationnel
    Base de données où les données sont organisées dans des tableaux
    à deux dimensions appelés « relations ».

Schéma
    Description des données dans un SGBD. Un SGBD contient au moins
    trois schémas : un **schéma conceptuel (ou logique)**, un
    **schéma physique**, et un ou plusieurs **schémas externes**.

Schéma conceptuel (ou logique)
    Description de toutes les relations comprises dans la base de
    données. Ces relations sont définies à l'aide d'un **langage de
    définition de données**.

Langage de définition de données
    Langage utilisé pour définir les schémas conceptuels et externes.
    Le langage de définition de données le plus répandu est SQL.

Schéma physique
    Description de la manière dont les relations sont stocké sur
    disque.

Schéma externe
    Descriptions des **vues** qui permettent un accès granulaire
    aux données selon les besoins et les permissions des
    utilisateur·rices.

Vue
    Relation synthétique calculée à partir des relations stockées
    dans la base de données.

