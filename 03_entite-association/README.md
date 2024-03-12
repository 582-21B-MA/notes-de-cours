# Modèle entité-association

Le modèle entité-association (en anglais, *entity-relationship* ou *ER*)
est une notation graphique qui permet de décrire un modèle de données en
termes d'**entités**, d'**associations** et d'**attributs**.

## Entité (*entity* ou *entity set*)

Une **entité** représente un type ou une classe d'« objets ». Ces objets
peuvent être concrets ou abstraits, animés ou inanimés, mais ils doivent
tous être caractérisés par les même **attributs**. Par exemple, dans le
contexte d'une école, `Élève`, `Enseignant·e`, `Cours` et `Classe`
peuvent être considéré·es comme des entités.

Un « objet » en particulier (le cours 582-21B, l'enseignant Maxime
Pigeon, etc.) est appelé « **instance** ». Le regroupement d'instances
qu'une entité représente est appelé « **population** ».

### Notation

On représente graphiquement une entité par un carré dans lequel est
indiqué son nom.

![Graphe entités](graphes/entite.svg)

## Attribut (*attribute*)

Un **attribut** permet de décrire une entité. Le choix des attributs
reflète le niveau de détail auquel les entités seront décrites. Chaque
attribut a un **type** (*domain*, en anglais) : numérique (*int* ou
*float*), texte (*char* ou *string*), date, booléen, etc. Par exemple,
l'entité `Élève` peut avoir les attributs `NumÉlève` (numérique), `Nom`
(texte) et `CoteR` (numérique).

### Notation

On représente graphiquement un attribut par un cercle ou un oval lié à
l'entité à laquelle l'attribut appartient, et dans lequel est indiqué le
nom de l'attribut. Si une entité contient beaucoup d'attributs, on peut
également lister ceux-ci à l'intérieur du carré de l'entité.

![Graphe attribut](graphes/attribut.svg)

## Association (*relationship*)

Les entités sont reliées entre-elles par des **associations** : un ou
une `Élève` *suit* un ou plusieurs `Cours` ; un ou une `Enseignant·e`
*enseigne* un `Cours`, etc. Vous remarquerez que les associations sont
souvent des verbes, alors que les entités sont souvent des noms.

### Notation

Une association est représentée graphiquement par un losange ou un
hexagone lié aux entités qui en font partie, et dans lequel est indiqué
son nom.

![Graphe association](graphes/association.svg)


### Association n-aire

L'association ci-haute est dite « **binaire** » car elle est formée de
deux entités. On peut généraliser ce nombre à plus de deux. On peut
ainsi décrire le fait que « les clients achètent des produits chez des
fournisseurs », ou que « enseignant·es enseignent des cours dans des
classes ».

![Graphe n-aire](graphes/n-aire.svg)

On qualifie de telles associations de « **n-aires** », où **n**
correspond au nombre d'entités. Les associations binaires sont les plus
fréquentes, suivis des associations ternaires (degré 3). Avant
d'utiliser une association de degré supérieur, on s'assurera que
celle-ci n'est pas décomposables.

### Association cyclique

Une association peut également être reliée à une seule entité. Ainsi, un
ou une `Élève` peut *donner du tutorat* à un ou une autre `Élève`. On
qualifie ce type d'associations de « **cyclique** ».

![Graphe cyclique](graphes/cyclique.svg)

### Attribut d'association

Une association peut elle aussi avoir des attributs. Par exemple,
l'association `réserve`, qui indique qu'un ouvrage de la bibliothèque
est réservé par un ou une élève, est caractérisé par l'attribut
`dateRéservation`. Cet attribut n'est spécifique ni à `Élève` ni à
`Ouvrage`, mais plutôt à l'association qui les unit.

#### Notation

Un attribut d'association est représenté graphiquement de la même façon
qu'un attribut d'entité, mais il est lié à l'association qu'il
caractérise.

![Graphe attribut d'association](graphes/attribut-association.svg)

## Contraintes d'intégrité

Les contraintes d'intégrité sont des propriétés que les données devront
respecter.

### Identifiant (*key*)

Chaque entité a un attribut ou un ensemble d'attributs dont les valeurs
permettent d'identifier de manière unique chacune de ses **instances**.
Par exemple, étant donné une valeur quelconque de `NumÉlève`, on a la
garantie qu'il n'y aura, à aucun moment, pas plus d'un ou une élève
possédant cette valeur.

Une entité peut posséder plusieurs **identifiants**. L'un d'eux est
déclaré **primaire** (*primary key*, en anglais), tandis que tous les
autres sont **secondaires** (*candidate keys*).

#### Notation

On identifie graphiquement l'identifiant primaire d'une entité en
sous-lignant le ou les attributs qui permettent de distinguer celle-ci.
Alternativement, on peut identifier ceux-ci textuellement.

![Graphe identifiant](graphes/identifiant.svg)

#### Identifiant hybride

Parfois, une entité est **faible**, c'est-à-dire qu'elle ne peut pas
être identifiée de manière unique par ses propres attributs. Par
exemple, disons qu'on veuille associer chaque élève à un ou plusieurs
parents dont nous connaissons seulement le nom et l'âge. Il est
impossible de garantir qu'il n'y aura jamais deux instances de l'entité
`Parent` avec les mêmes valeurs pour ces attributs.

Dans ce cas, on peut utiliser un **identifiant hybride** composé de un
ou plusieurs attributs locaux jumelés à l'identifiant primaire d'une
entité voisine. Une restriction s'applique toutefois : l'entité faible
doit participer à une association de type « un-à-un » (voir ci-bas) avec
l'entité voisine.

##### Notation

Une entité faible est représentée graphiquement soit par une bordure
double, soit par une bordure plus grasse. L'association qui relie
l'entité faible à l'entité voisine à laquelle elle appartient héritera
également du même style. Alternativement, on peut utiliser l'identifiant
de l'entité voisine comme attribut de l'entité faible. Celui-ci sera
nommé selon la syntaxe suivante : « Entité.propriété ». Par exemple,
`Élève.NumÉlève`.

![Graphe hybride](graphes/hybride.svg)

### Cardinalité

En plus de représenter les entités et les associations d'un modèle de
données, il est possible d'indiquer à combien d'instances d'une
association les instances d'une entité peuvent et doivent participer.
Cette contrainte, nommée **cardinalité**, est représentée par deux
propriétés : la **classe fonctionnelle** et le caractère **obligatoire
ou facultatif** d'une association.

#### Notation

![Graphe cardinalité](graphes/cardinalite.svg)

Le diagramme ci-haut se lit comme suit : Une instance de l'entité `E1`
est associée à au moins `MIN1` et au plus `MAX1` instances de l'entité
`E2`. Pareillement, une instance de l'entité `E2` est associée à au
moins `MIN2` et au plus `MAX2` instances de l'entité `E2`.

Généralement, la valeur de `MIN` est soit `0` (facultative) soit `1`
(obligatoire). La valeur de `MAX`, quant-à-elle, est soit `1` (une
instance d'association) soit `N` (plusieurs instances d'association).

#### Classe fonctionnelle

Cette propriété décrit le nombre maximum d'instances de l'entités A pour
chaque instance de l'entité B, et vice versa. Il existe trois **classes
fonctionnelles** : **un-à-plusieurs**, **un-à-un** et
**plusieurs-à-plusieurs**.

##### Un-à-plusieurs (1:N)

Dans le contexte d'un cours, un ou une enseignante enseigne à
**plusieurs** élèves, et une ou une élève ont **un ou une** seule
enseignante. Il s'agit donc d'une association **un-à-plusieurs**.

![Graphe un-à-plusieurs](graphes/un-a-plusieurs.svg)

##### Un-à-un (1:1)

Dans le contexte d'une école, un casier appartient à **un ou une**
élève, et un ou une élève possède **un** seul casier. Il s'agit donc
d'une association **un-à-un**.

![Graphe un-à-un](graphes/un-a-un.svg)


##### Plusieurs-à-plusieurs (N:N)

Dans le contexte d'un programme scolaire, un ou une élève suit
**plusieurs** cours, et un cours peut être suivi par **plusieurs**
élèves. Il s'agit donc d'une association **plusieurs-à-plusieurs**.

![Graphe plusieurs-à-plusieurs](graphes/plusieurs-a-plusieurs.svg)

##### Important

On ne peut caractériser la classe fonctionnelle d'une association
qu'après avoir répondu à deux question :

1.  Combien d'instances de l'entités A il y a-t-il pour chaque instance
    de l'entité B ?
2.  Combien d'instances de l'entités B il y a-t-il pour chaque instance
    de l'entité A ?

#### Obligatoire ou facultatif

Certaines associations sont **obligatoires** pour le ou les entités qui y
participent. Par exemple, un cours doit obligatoirement être enseigné par
un ou une enseignante. De la même façon, certaines associations sont
**facultatives** : une ou une enseignante peut ne pas enseigner de cours
(parce qu'il ou elle est en congé de maladie, par exemple).

![Graphe obligatoire et facultatif](graphes/obligatoire-facultatif.svg)

## Bibliographie

-   Jean-Luc Hainaut. Bases de données. Concepts, utilisation et
    développement. Dunod, 2018.
-   Raghu Ramakrishnan, Johannes Gehrke. Database Management Systems.
    McGraw-Hill, 2003.
