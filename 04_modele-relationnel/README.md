# Modèle relationnel

Introduit en 1970 par Edgar F. Codd, alors chercheur chez IBM, le modèle
relationnel propose de structurer les données d'une base de données à
l'aide de deux concepts élémentaires : le *domaine* et la *relation*.
L'organisation des données dans le modèle relationnel se fait sans
considérations technologiques.

## Concepts de base

![Représentation d'une relation](images/table.gif)

### Ensemble (*set*)

Au sens mathématique du terme : rassemblement d'éléments *distincts*.
Autrement dit, tous les éléments d'un **ensemble** sont uniques.

### Domaine

**Ensemble** des valeurs possibles pour un seul attribut. Par exemple,
étant donné la proposition « un ou une élève a un numéro d'élève, un
nom, et une cote R », le domaine de l'attribut *cote R* est 1, 2, ...,
50.

### Uplet (*tuple*)

Collection ordonnée et finie d'éléments. Un uplet contenant deux
éléments (2-uplet) est appelé « doublet ». Un 3-uplet est appelé
« triplet ». Un 4-uplet est appelé « quadruplet ». Et cetera.

### Relation

**Ensemble** d'**uplets** dont chaque valeur appartient à un
**domaine**. Tous les uplets d'une même relation contiennent exactement
*une* valeur (ni plus, ni moins) pour chaque domaine.

### Schéma

Description d'une **relation**. Un **schéma** contient le nom de la
relation ainsi que le nom et le type de ses attributs. La notation
suivante est souvent utilisée pour représenter le schéma d'une
relation :

```
Students(sid: int, name: string, gpa: real)
```

Le schéma ci-haut indique que la relation nommée `Students` possède
l'attribut `sid` de type `int`, l'attribut `name` de type `string`, et
l'attribut `gpa` de type `real` (nombre décimal).

### Table

Représentation matérielle d'une **relation**. Par exemple, une instance
de la relation `Students` dont le **schéma** est décrit ci-haut peut être
représentée par la **table** suivante :

| sid  | name    | gpa |
| ---- | ------- | --- |
| 5000 | Dave    | 3.3 |
| 5368 | Madayan | 3.8 |
| 5602 | Xuan    | 3.5 |

Dans la table ci-dessus, chaque colonne représente un domaine, et
chaque ligne représente un uplet. L'ordre des lignes est inconséquent,
mais celui des colonnes est significatif. Nous pourrions aussi affirmer
que l'entête de la table correspond au schéma de la relation, et que son
corps correspond à l'extension de la relation. Dans tous les cas, il
faudra se rappeler que cette représentation n'est pas essentielle
au modèle relationel en tant que tel ; ce n'est qu'une illustration.

### Degré (ou arité)

Nombre de **domaines** pour une **relation** donnée. Une relation avec
un seul domaine (degré 1) est appelé « unaire ». Une relation de degré 2
est appelée « binaire ». Une relation de degré 3 est appelée « ternaire
». Et cetera.

### Cardinalité

Nombre de **uplet** pour une **relation** donnée.

## Bibliographie

-   Edgar F. Codd. A Relational Model of Data for Large Shared Data
    Banks. Communications of the ACM, 1970.
-   Jean-Luc Hainaut. Bases de données. Concepts, utilisation et
    développement. Dunod, 2018.
-   Raghu Ramakrishnan, Johannes Gehrke. Database Management Systems.
    McGraw-Hill, 2003.