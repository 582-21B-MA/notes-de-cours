# SQL DML

Le sous-langage DML (*Data Manipulation Language*) est la partie la plus
riche du langage SQL. Il permet d'extraire de données et de modifier
celles-ci.

## Extraire des données

La requête `SELECT` permet d'extraire les données d'une base de données.
Le résultat est présenté sous la forme une table fictive. Une requête
`SELECT` contient généralement trois parties :

-   la clause `SELECT`, qui précise les colonnes constituant chaque
    ligne du résultat,
-   la clause `FROM`, qui indique la ou les tables desquelles le
    résultat tire ses valeurs,
-   la clause `WHERE`, qui spécifie la condition de sélection que
    doivent satisfaire les lignes qui fournissent le résultat.

La requête la plus simple consiste à extraire les valeurs de certaines
colonnes. Par exemple, la requête suivante retourne les valeurs de `sid`,
`name` pour toutes les lignes de la table `Students` :

```sql
SELECT sid, name
FROM   Students;
```

La clause `SELECT` suivie de `*` permet d'extraire les données de toutes
les colonnes :

```sql
SELECT * FROM Students;
```

### Conditions de sélection

Il est possible d'extraire seulement les données qui satisfont certaines
conditions de sélection. Pour ce faire, on utilise la clause `WHERE`
suivie de la condition de sélection en question. Par exemple, la requête
suivante extrait seulement les lignes dont la valeur de l'attribut `gpa`
est plus grande que 3.5 :

```sql
SELECT sid, name, gpa
FROM   Students
WHERE  gpa > 3.5;
```

SQL dispose des comparateurs suivants :

| Relation           | Comparateur |
| ------------------ | ----------- |
| égal à             | `=`         |
| plus grand que     | `>`         |
| plus petit que     | `<`         |
| différent de       | `<>`        |
| plus grand ou égal | `>=`        |
| plus petit ou égal | `<=`        |

#### Présence ou absence d'une valeur

Les formes spéciales `IS` et `IS NOT` sont utilisé pour exprimer l'égalité
ou la non égalité dans le cas des valeurs `NULL` :

```sql
SELECT sid, name, gpa
FROM   Students
WHERE  gpa IS NOT NULL;
```

#### Appartenance à une liste

On utilise le mot-clé `IN` pour vérifier si les valeurs font parties de
la liste donnée. La requête suivante, par exemple, extrait les lignes
dont la valeur de la colonne `major` est soit « Computer Science » soit
« Philosophy » soit « Literature » :

```sql
SELECT sid, name, major
FROM   Students
WHERE  major IN ('Computer Science', 'Philosophy', 'Literature');
```

La forme inverse est également possible afin d'extraire les lignes dont
la valeur de la colonne `major` n'est *pas* contenue dans la liste :

```sql
SELECT sid, name, major
FROM   Students
WHERE  major NOT IN ('Computer Science', 'Philosophy', 'Literature');
```

#### Intervalle

Il possible d'extraire seulement les lignes dont les valeurs pour
certaines colonnes sont comprise dans un intervalle à l'aide du mot-clé
`BETWEEN`. Ici-bas, par exemple, le résultat contiendra les lignes dont
la valeur de `gpa` est entre 3.0 et 3.5 :

```sql
SELECT sid, name, gpa
FROM   Students
WHERE  gpa BETWEEN 3.0 AND 3.5;
```

#### Masques

Un masque est une chaîne de caractères qui agissent comme un filtre. Il
désigne la structure générale des valeurs qui satisfont la condition. Le
mot-clé `LIKE` est utilisé pour introduire un masque. Au sein du masque,
le caractère `_` désigne un caractère quelconque, et `%` désire une
suite de caractères quelconques, éventuellement vide. Par exemple,
la requête suivante sélectionne les lignes dont la valeur de la colonne
`major` contient la chaîne « science » :

```sql
SELECT sid, name, major
FROM   Students
WHERE  major LIKE '%Science%';
```

Pour rechercher les caractères `%` et `_`, il suffira de les préfixer
dans le masque par un caractère spécifié dans la clause `ESCAPE` :

```sql
SELECT sid, name, login
FROM   Students
WHERE  login LIKE 'ava\_%' ESCAPE '\';
```

Ci-haut, la masque sélectionne les valeurs de `login` qui commencent par
la chaîne « ava_ ».

#### Expression composée

La condition de sélection introduite par la clause `WHERE` peut être
constituée d'une expression booléenne de conditions élémentaires
(opérateurs `AND`, `OR`, `NOT` et parenthèses). Par exemple, la requête
suivante retient les lignes pour lesquelles, simultanément, la valeur de
`major` est « literature », et celle de `gpa` est supérieure à 3.00 :

```sql
SELECT sid, name, gpa
FROM   Students
WHERE  major = 'literature' AND gpa > 3.0;
```

L'utilisation des parenthèses permet de former des conditions plus
élaborées La requête suivante retient les lignes pour lesquelles la
valeur de `gpa` est supérieure ou égale à 3.50, et la valeur de `major`
est soit « Literature » soit « Philosophy » :

```sql
SELECT sid, name, gpa
FROM   Students
WHERE  gpa >= 3.50 AND (major = 'Literature' OR major = 'Philosophy');
```

### Lignes dupliquées

Par défaut, le résultat d'une requête monotable contient toutes les
lignes qui satisfont la condition de sélection. Il se peut donc que le
résultat contienne plusieurs lignes identiques si les colonnes
sélectionnées ne sont pas des identifiants. On éliminera les lignes en
double par la clause `DISTINCT` :

```sql
SELECT DISTINCT major
FROM   Students
```

### Ordre des lignes

L'ordre des lignes d'une table est arbitraire. Il est cependant possible
d'imposer un ordre au résultat d'une requête via la clause `ORDER BY`,
comme illustré par la requête suivante :

```sql
SELECT gpa, name
FROM   Students
ORDER BY gpa DESC, name ASC;
```

Ci-haut, les lignes apparaîtront par valeurs décroissantes de `gpa`, et
au sein de chaque groupe de même valeur de `gpa`, par valeurs
croissantes de `name`.

### Fonctions

SQL offre une panoplie de fonctions permettant de dériver des valeurs à
partir, entre autres, des valeurs des colonnes des lignes extraites. Des
expressions faisant usage de fonctions peuvent apparaître dans la clause
`SELECT` et dans la clause `WHERE`, mais aussi, dans les instructions
`UPDATE` et `INSERT`.

#### Fonctions numériques

Il s'agit des quatre opérateurs arithmétiques classiques : `+`, `-`, `*`
et `/`.

#### Fonctions de chaînes de caractères

Ces fonctions renvoient soit une chaîne, soit un nombre entier :

-   `CHAR_LENGTH(ch)` : donne le nombre de caractères de la chaîne
    `ch`.
-   `POSITION(ch1 IN ch2)` : donne la position de chaîne `ch1` dans la
    chaîne `ch2` ; 1 si `ch1` est vide et 0 si `ch1` n'apparaît pas
    dans `ch2`.
-   `ch1 || ch2` : construit une chaîne composée de la concaténation
    (mise bout à bout) des chaînes `ch1` et `ch2`.
-   `LOWER(ch)` et `UPPER(ch)` : construit une chaîne formée des
    caractères de `ch` transformés respectivement en minuscules et
    majuscules.
-   `SUBSTRING(ch FROM i FOR l)` : construit une chaîne formée des `l`
    caractères de la chaîne `ch` partant de la position `i` ;.
-   `TRIM(e c FROM ch)` : supprime les caractères `c` à l'extrémité
    `e` de la chaîne `ch` ; les valeurs de `e` sont `LEADING`,
    `TRAILING` et `BOTH`. Le format simplifié `trim(ch)` correspond à
    `TRIM(BOTH ' ' FROM ch)`.

Par exemple, la requête suivante retourne les valeurs de la colonne
`name` transformées en minuscules :

```sql
SELECT LOWER(name) AS Name
FROM Students;
```
### Fonctions agrégatives

Il existe également des fonctions prédéfinies qui donnent une valeur
agrégée calculée pour les lignes sélectionnées :

-   `COUNT(*)` donne le nombre de lignes trouvées,
-   `COUNT(nom-colonne)` donne le nombre de valeurs de la colonne,
-   `AVG(nom-colonne)` donne la moyenne des valeurs de la colonne, 
-   `SUM(nom-colonne)` donne la somme des valeurs de la colonne,
-   `MIN(nom-colonne)` donne le minimum des valeurs de la colonne, 
-   `MAX(nom-colonne)` donne le maximum des valeurs de la colonne.

La requête suivante, par exemple, fournit une table qui contient la
moyenne, la valeur minimum et maximum, ainsi que le nombre de lignes de
la colonne `gpa` pour les élèves qui étudient la littérature :

```sql
SELECT major    AS Major,
       AVG(gpa) AS Average,
       MIN(gpa) AS Minimum,
       MAX(gpa) AS Maximum,
       COUNT(*) AS Students
FROM   Students
WHERE  major = 'Literature';
```
### Données groupées

La plupart des requêtes précédentes produisent des lignes qui
correspondent *une pour une* avec les lignes d'une table de la clause
FROM. Il est toutefois possible d'extraire des informations sur les
concepts *latents* d'une table.

Pour ce faire, on utilise la clause `GROUP BY`, qui permet de regrouper
ou de classer les lignes selon leur valeur pour une colonne donnée. Par
exemple, la requête suivante donne, pour chaque sujet d'étude, le nom de
celui-ci, le nombre d'élève, et la moyenne des valeur de la colonne
`gpa` :

```sql
SELECT   major    AS Major,
         COUNT(*) AS Students,
         AVG(gpa) AS Average
FROM     Students
GROUP BY major;
```

Attention, on ne peut trouver dans la cause `SELECT` que des éléments
qui définissent *une seule valeur par groupe*. Il aurait donc été
impossible, par exemple, d'ajouter les colonnes `name` et `login`.

### Sélection de groupe

Des conditions de sélection peuvent être imposées aux groupes à
sélectionner. Elles seront exprimées dans une clause `HAVING`, ce qui
évite toute confusion avec la clause `WHERE` qui, elle, s'applique aux
lignes avant groupement. Dans la requête suivante, on ne retient par
exemple que les groupes d'au moins trois élèves :

```sql
SELECT   major    AS Major,
         COUNT(*) AS Students,
         AVG(gpa) AS Average
FROM     Students
GROUP BY major
HAVING   COUNT(*) >= 3;    
```

## Jointure

Jusqu'ici, nous avons extrait des données issues d'une seule table, mais
il est aussi possible de *coupler* les lignes de deux ou plusieurs
tables afin d'en extraire des données corrélées. Pour ce faire, on
précise d'abord les tables sources dans la clause `FROM`, puis on
définit une **condition de jointure** dans la clause `WHERE`.

Par exemple, la requête suivante retourne une table dont la colonne
`name` est tirée de la table `Students` et dont la colonne `title` est
tirée de la table `Projects` :

```sql
SELECT S.name AS Name, P.title AS "Project title"
FROM   Students S, Projects P
WHERE  S.sid = P.sid;
```

Conceptuellement, le résultat est obtenu comme suit :

1.  Une table synthétique est construite en couplant chaque ligne de
    la première table à chaque ligne de la seconde (clause `FROM`).
    La table ainsi créée contient autant de colonnes que la somme des
    colonnes des deux tables, et autant de lignes que le produit entre
    le nombre de ligne de la première table et le nombre de ligne de la
    deuxième.
2.  On sélectionne ensuite parmis les lignes de la table synthétique
    celles qui vérifient la condition de jointure, ainsi que les autres
    conditions éventuelles (clause `WHERE`).
3.  On retient enfin que les colonnes demandées dans la clause `SELECT`.

### Lignes célibataires

Vous remarquerez que les élèves qui ne sont pas associé·es à un projet
n'apparaissent pas dans le résultat. Nous qualifierons de
**célibataires** les lignes qui leur correspondent. En toute généralité,
la jointure de deux tables exclut dans chacune d'elle les lignes
célibataires, c'est-à-dire les lignes qui n'ont pas de correspondant
dans l'autre table.

### Jointure externe

Il existe une variante spéciale de la jointure, appelée **jointure
externe** (*outer join*, en anglais), dont la particularité est de
compléter la jointure naturelle par les lignes célibataires d'une ou des
deux tables sources :

```sql
SELECT S.name AS Name, P.title AS "Project title"
FROM   Students S LEFT OUTER JOIN Projects P
ON     S.sid = P.sid;
```

Le terme `LEFT OUTER JOIN` indique qu'on inclut les lignes célibatires
de la table de gauche dans le résultat. Il existe aussi les variantes
`RIGHT OUTER JOIN` et `FULL OUTER JOIN`.

## Modifier des données

### Ajouter une ligne

Pour ajouter une ligne constituée de valeurs nouvelles, on utilise
l'instruction `INSERT VALUES` :

```sql
INSERT INTO Students (sid, name, gpa)
VALUES      (53045, 'Tanveer', 3.8);
```

#### Marqueur `NULL`

Des circonstances particulières peuvent faire qu'il faille procéder à
l'enregistrement d'une ligne alors que certaines données ne sont pas
connues. Dans ces cas, on peut assigner à la colonne en question le
*marqueur* `NULL`, ou simplement omettre celle-ci (ce qui revient au
même) :

```sql
INSERT INTO Students (sid, name)
VALUES      (53045, 'Tanveer');
```

Les colonnes facultatives sont délicates à manipuler. Il est préférable
de les éviter lorsque possible.

### Supprimer une ligne

Les requêtes de suppression `DELETE` porte sur un sous-ensemble de
lignes désignées par une clause `WHERE`. Par exemple, la requête
suivante supprime la ligne dont la valeur de `sid` est égale à 1.

```sql
DELETE FROM Students
WHERE sid = 1;
```

### Modifier une ligne

On peut modifier les lignes qui satisfont une condition de sélection à
l'aide des clauses `UPDATE` et `SET`. Par exemple, la requête suivante
change la valeur de `gpa` de la ligne dont la valeur de `sid` est 2 :

```sql
UPDATE Students
SET gpa = 1.5
WHERE sid = 2;
```
