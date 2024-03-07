# Étapes du processus de conception d'une base de données

Le processus de conception d'une base de données peut être divisé en six
étapes chronologiques. Notez que plus le processus avance, plus la base
de données se concrétise. Les premières étapes, davantage abstraites et
subjectives, sont plus faciles à reprendre.

## 1. Analyse des besoins

En premier lieu, avant de commencer la conception à proprement dit, il
faut comprendre et lister quelles sont les données qui doivent être
stockées dans la base de données, quelles sont les applications qui
seront connectées à celle-ci, et quelles seront les opérations les plus
fréquentes. Autrement dit, quels sont les besoins des utilisateur·rices
de la base de données.

Ce processus nécessite généralement des entretiens avec les une
utilisateur·rices, étude de l'environment opérationnel où sera utilisée
la base de données, ainsi qu'une analyse des solutions de stockage
existantes qui seront remplacées le nouveau système de gestion de base
de données (SGBD).

## 2. Conception du modèle sémantique

Les informations recueillies lors de l'analyse des besoins sont ensuite
utilisées pour concevoir un modèle sémantique de la base de données. Le
modèle sémantique le plus utilisé est le modèle entité-association (en
anglais, *entity-relationship* ou *ER*).

L'objectif de cette étape est de produire une description simple et
visuelle des données. Cela facilite les échanges entre les personnes
impliquées dans le processus de conception, même si elles n'ont pas de
formation technique.

Quoique abstrait, le modèle sémantique doit être suffisamment précis
pour qu'il soit possible de la traduire ensuite en un schéma qui sera
pris en charge par le SGBD.

## 3. Conception du schéma logique

Une fois le modèle sémantique réalisé, il faut choisir un SGBD, et
convertir la représentation visuelle de la base de données en schéma
logique (aussi appelé schéma conceptuel). Le format de se schéma dépend
du type de SGBD choisi. Si celui-ci est de type « relationnel », comme
c'est souvent le cas, alors le modèle sémantique doit être traduit en
relations (aussi appelées « tables »).

## 4. Normalisation des relations

La quatrième étape consiste à assurer la qualité des relations, un
processus appelé « normalisation ». Le but est d'éviter le redondance et
l'incomplétude des données. Pour ce faire, le schéma logique est révisé
pour respecter une série de « formes normales ».

Les formes normales sont des contraintes prédéfinies auxquelles une base
de données doit se conformé afin d'être considéré comme étant robuste.
Les formes normales sont classées selon leur degré de restriction : 1FN
(première forme normale), 2FN, 3FN, FNBC (forme normale de Boyce-Codd),
4FN, 5FN, et ainsi de suite. En pratique, la première et la deuxième
forme normale sont nécessaires pour avoir un modèle relationnel juste.
L'usage est de respecter au minimum le troisième forme normale.

## 5. Conception du schéma physique

Durant cette étape, le schéma logique est validé en fonction des charges
de travail typiques auxquelles le SGBD sera soumis. Parfois, afin de
respecter les critères de performance, il peut être nécessaire de créer
des index (une sorte de raccourcis vers certaines données fréquemment
utilisées) ou de réviser le schéma logique.

## 6. Conception des schémas externes

Enfin, il faut prendre en compte les applications qui doivent se
connecter au SGBD. Pour chacune d'elles, des vues sont créées selon les
parties de la base de données qui doivent ou non être accessibles.

## Bibliographie

-   Raghu Ramakrishnan, Johannes Gehrke. Database Management Systems.
    McGraw-Hill, 2003.
