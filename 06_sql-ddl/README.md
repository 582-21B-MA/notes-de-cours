# SQL DDL

SQL (*Structured Query Language*, parfois prononcé « sequel ») est un
langage déclaratif qui permet, d'une part, de définir des structures de
données (les composants du schéma), et, de l'autre, de manipuler
celles-ci. Le sous-langage qui s'occupe de la définition se nomme
« *DDL* » (*Data Definition Language*), tandis que celui qui se charge
de la manipulation est appelé « *DML* » (Data Manipulation Language).

Une instruction SQL, appelée **requête**, décrit une opération que le
système de gestion de base de données (SGBD) doit exécuter. Une requête
peut être introduite par le biais d'une interface en ligne de commande,
auquel cas le résultat de celle-ci apparaît dans le terminal. Une
requête peut aussi être faite à partir d'un programme. Dans ce cas, le
résultat de la requête est rendu disponible directement au sein du
programme.

## Nommage

Le noms des tables, des colonnes et autres objets SQL est soumis à
certaines règles :

1.  Un nom ne peut pas contenir d'espace ni d'accent.
2.  Seuls les symboles `#`, `$` et `_` sont permis.
3.  Un nom ne peut pas commencer par un chiffre.
4.  Les mots-clés SQL ne peuvent pas être utilisés.
5.  Les lettres majuscules et minuscules sont équivalentes (« taux »
    et « TAUX » sont considéré comme identiques).

## Créer un table

La requête `CREATE TABLE` produit une table vide dont le schéma est
conforme aux indications données. On y spécifie le nom de la table et,
pour chaque colonne, son nom et le type de ses valeurs.

```sql
CREATE TABLE Students (
    sId   INTEGER,
    name  CHAR(30),
    gpa   REAL
);
```

Pour prendre en compte le fait que la table existe déjà, on utilisera
les formes `CREATE OR REPLACE TABLE Students` ou `CREATE TABLE IF NOT
EXISTS Students`.

## Types

SQL offre plusieurs types de données :

-   `SMALLINT` : entiers signés courts (par ex. 16 bits)
-   `INTEGER` ou `INT` : entiers signés longs (par ex. 32 bits)
-   `NUMERIC(p,q)` : nombres décimaux de `p` chiffres dont `q` après le
    point décimal; si elle n'est pas mentionnée, la valeur de `q` est 0
-   `DECIMAL(p,q)` : nombres décimaux d'au moins p chiffres dont `q`
    après le point décimal ; si elle n'est pas mentionnée, la valeur
    de `q` est 0
-   `FLOAT(p)` ou `FLOAT` ou `REAL` : nombres en virgule flottante
    d'au moins `p` bits significatifs
-   `CHARACTER(p)` ou `CHAR` : chaîne de longueur fixe de `p` caractères
-   `CHARACTER VARYING` ou `VARCHAR(p)` : chaîne de longueur variable
    d'au plus `p` caractères
-   `BIT(p)` : chaînes de longueur fixe de `p` bits
-   `BIT VARYING` : chaînes de longueur variable d'au plus `p` bits
-   `DATE` : dates (année, mois et jour)
-   `TIME` : instants (heure, minute, seconde, éventuellement 1000e de
    seconde)
-   `TIMESTAMP` : date + temps
-   `INTERVAL` : intervalle en années/mois/jours entre dates ou en
    heures/minutes/ secondes entre instants

## Contraintes d'intégrité

### Colonne obligatoire

Par défaut, toutes les colonnes d'une table sont **facultatives**. On
peut cependant imposer l'interdiction d'assigner la valeur `NULL` à une
colonne avec la clause `NOT NULL` :

```sql
CREATE TABLE Students (
    sId   INTEGER   NOT NULL,
    name  CHAR(30)  NOT NULL,
    gpa   REAL
);
```

Ci-dessus, le SGBD garantit le caractère **obligatoire** des colonnes
`sId` et `name`. Toute tentative d'insérer une ligne qui ne possède
pas de valeur pour ces colonnes sera automatiquement rejetée.

### Identifiant

Une des fonctions qui peut être associées à une colonne est
l'identification univoque de l'objet représenté. Une colonne constitue
un **identifiant** de sa table si, à tout instant, il ne peut exister
plus d'une ligne qui possède la même valeur pour cette colonne. Un
identifiant peut également comprendre plusieurs colonnes. On parle alors
d'**identifiant composite**.

Une table peut contenir plusieurs identifiants. Parmis ceux-ci, un seul
sera déclaré comme **identifiant primaire** avec la clause `PRIMARY
KEY`. Les autres, appelés **identifiants secondaires** (*candidate
keys*, en anglais), seront déclarés avec le prédicat `UNIQUE` :

```sql
CREATE TABLE Students (
    sId    INTEGER   NOT NULL,
    login  CHAR(20)  NOT NULL,
    name   CHAR(30)  NOT NULL,
    gpa    REAL,

    UNIQUE (login),
    PRIMARY KEY (sId)
);
```

Étant donné la définition ci-haute, le SGBD rejettera automatiquement
toute tentative d'insertion d'une ligne dont la valeur de `login` ou
celle de `sId` est déjà présente dans la table. Cette contrainte
d'unicité nous assure que les valeurs de ces colonnes seront toujours
distinctes.

La notation suivante est équivalente :

```sql
CREATE TABLE Students (
    sId    INTEGER   PRIMARY KEY,
    login  CHAR(20)  NOT NULL  UNIQUE,
    name   CHAR(30)  NOT NULL,
    gpa    REAL
);
```

### Clé étrangère

Pour modéliser certaines situations, il est parfois nécessaire pour une
table (dite **table source**) de référer une ligne dans une autre table
(dite **table cible**). Pour ce faire, on utilise une **clé étrangère**
(*foreign key*, en anglais), laquelle est la copie d'un identifiant de
la table cible.

Supposons que des élèves travaillent sur des projets. Pour chaque
projet, nous savons le titre et la note finale. Plusieurs élèves peuvent
travailler sur le même projet, et chaque projet possède un nom unique.
Il est possible de modéliser cette situation à l'aide de la table
suivante :

```sql
CREATE TABLE Students (
    sId      INTEGER,
    name     CHAR(30),
    project  CHAR(100),
    grade    INTEGER,

    PRIMARY KEY (sId)
);
```

Toutefois, cela rendrait périlleuse toute modification. Pour changer la
note d'un seul projet, par exemple, il faudrait modifier plusieurs
lignes. Plutôt, nous **décomposerons** la relation en deux tables :

```sql
CREATE TABLE Students (
    sId    INTEGER,
    name   CHAR(30),
    pName  CHAR(100),

    PRIMARY KEY (sId),
    FOREIGN KEY (pName) REFERENCES Projects
);

CREATE TABLE Projects (
    name   CHAR(100),
    grade  INTEGER,

    PRIMARY KEY (name)
);
```

La table `Students` contient une colonne `pName` qui réfère la table
cible `Projects`. Pour créer cette référence, nous utilisons le mot-clé
`FOREIGN KEY`, suivi du nom de la clé étrangère, suivi du mot-clé
`REFERENCES`, suivi du nom de la table cible. Par défaut, l'identifiant
visé par la clé étrangère dans la table cible est l'identifiant primaire
de celle-ci.

La notation suivante est équivalente pour la table `Students` :

```sql
CREATE TABLE Students (
    sId    INTEGER    PRIMARY KEY,
    name   CHAR(30),
    pName  CHAR(100)  REFERENCES Projects,
);
```

Puisque la clé étrangère est une copie de l'identifiant cible, elle doit
être formée de colonnes en même nombre et de mêmes types que
l'identifiant cible. Une clé étrangère dont l'identifiant cible est
composite est elle-même composite.