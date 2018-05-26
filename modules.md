<a id="top"></a>
## la modularité

La modularité est devenue essentielle dans le domaine du développement
informatique.  Créer, en 2018, un langage qui ne serait pas modulaire serait
-- à mon sens -- une aberration totale.

Dés qu'il est question de modularité, il y a deux aspects à prendre en
compte :
- l'intégration d'une fonctionnalité au sein d'un module et
- l'importation d'un module.

Cependant, l'expérience démontre à loisir que ce n'est pas parce que nous
avons besoin d'une fonctionnalité particulière issue d'un module bien spécifique
que nous avons **forcément** besoin de **l'intégralité** de ce module.

Qu'il s'agisse d'intégration à un module ou d'importation d'un module, une
approche plus "sélective" doit être envisagée.


### Des modules et des sous-modules

L'étude de bibliothèques de renom écrites en C++ et de leur histoire le
démontre largement : l'objectif poursuivi par une bibliothèque -- quelle
qu'elle soit -- évolue énormément au fil du temps; mais toujours dans la
même direction : vers une complexité accrue.

Vers la capacité de rendre des services de plus en plus nombreux,
s'éloignant de plus en plus de leur objectifs originel.

Qt en est le parfait exemple, dans le sens où  elle a été développée à
la base pour  permettre le développement d'interfaces graphiques mais
qu'elle "étoffée" au fil du temps pour répondre à de plus en plus de
besoin, comme

- la communication avec des bases de données
- la gestion de fichiers XML
- l'impression de documents
- l'intégration de graphismes 3D
- les protocoles de communication W3C
- j'en passe et de meilleures.

Comme la plupart de ces fonctionnalités sont indépendantes entre elles,
Qt a du mettre en place un système q'il appelle lui-même des "modules",
et qui permet de n'utiliser que les fonctionnalités dont on a besoin.

De la même manière, si l'on considère la STL du C++ comme "un module général",
nous nous rendons compte que des "modules secondaires" sont apparus au fils
du temps, au travers d'espaces de noms comme `std::placeholders`, `std::chrono`
ou `std::experimental`.

La capacité du langage à supporter des "sous modules" (comprenez: des modules
faisant "officiellement" partie d'un module plus "général" mais proposant
de fonctionnalités totalement indépendantes) est donc une priorité

### des groupes et des sous-groupes

A partir du moment où la capacité de créer des "sous-modules" est envisagée,
nous devons également envisager la possibilité de "regrouper" plusieurs
sous-modules de manière à en permettre l'utilisation de manière coordonnée.

Si l'on prend la bibliothèque standard du C++ comme exemple, nous nous
rendons compte qu'il existe plusieurs fonctionnalités associées à la notion
de flux:

- les "entrées / sorties standard", généralement associée au clavier et
  à l'écran dans le cadre d'un ordinateur
- les fichiers qui peuvent eux aussi servir aux entrées (lecture) et
  aux sorties (écriture)
- les flux de conversion -- `std::istringstream` et `std::ostringstream`
   -- même si leur utilisation tend à disparaitre.

Ces fonctionnalités sont totalement indépendantes les unes des autres,
dans le sens où ce n'est pas parce que vous utiliser la notion de fichier
dans une unité de compilation que vous disposez forcément de la possibilité
d'avoir recours aux entrées et sorties standards, aux possibilité de
manipulation de flux (cf le fichier d'en-tête `iomanip` )ou aux flux de
conversion.

Cependant, nous pourrions parfaitement vouloir disposer de "tout ce qu'il
faut" pour pouvoir manipuler "des entrées / sortie" (sans distinction).

De la même manière, si l'on reprend Qt comme exemple, ce n'est pas parce que
nous souhaitons créer une interface graphique que nous aurons forcément
besoin de recourir aux notions de dessin (et inversement):

Pour créer une interface graphique, nous utilserons sans doute des Label,
des TextEdit, des ComboBox et autres "widgets" graphiques, alors que pour
une application de dessin, nous aurons d'avantage recours aux GraphicsScene,
GraphicsView, GraphicsItem et autres GraphicsObjects.

Il pourrait être intéressant -- surtout dans le cadre de l'utilisaiton de
modules -- de fournir un moyen de regrouper toutes les fonctionnalités qui
vont bien ensemble : tous les "Widgets" d'un coté, tous les "GraphicsObjects"
de l'autre et, pourquoi pas, de regrouper ces deux ensembles de fonctionnalités
qui vont "si bien ensembles".

des notions de groupes et de sous-groupes permettron d'automatiser ces
regroupements.

### intégrer des bibliothèques "étrangères"

J'ai bien conscience que la tentative de créer un nouveau langage à usage
général est un pari risqué à l'heure actuelle, surtout si l'objectif
l'objectif avoué est d'arriver à supplanter des langages implantés depuis
plus de trente ans comme C ou C++.

Car il semble uthopique d'espérer que les utilisateur de C ou de C++ se
tournent vers ce nouveau langage si la première chose qu'ils doivent faire
consiste recréer l'ensemble des bibliothèques et des outils qu'ils ont
l'habitude d'utiliser.

Par contre, on **peut** espérer qu'ils essayent le langage si "tout ce qu'ils
ont à faire" pour pouvoir utiliser leurs bibliothèques favorites (écrites
en C ou en C++) consiste à lancer une procédure proche de

```Bash

    COMPILER --createAlienModule c++ <name> <libXXX.a> <where to find headers>

```

et qu'il leur suffit ensuite d'une ligne proche de

```
    importLib c++ <name> <en-tête1> <en-tête2> ... <en-têteN>

```

dans leur code pour pouvoir l'utiliser.

Accessoirement, la capacité de faire la procédure inverse -- c'est à dire
de créer des fichiers d'en-tête en C et/ ou en C++ à partir du système
de modules de notre langage -- pourrait s'avérer particulièrement
intéressante pour inciter les gens à l'utiliser'.

Bien sur, le temps nécessaire à la création de ce "module étranger" sera
sans doute décisif.  Mais ca... nous nous en occuperons plus tard.

> **note** Il n'est nullement question ici de fournir la moindre compatibilité
en termes de **code** à notre langage, mais bien de fournir une compatibilité
en termes d' **ABI**

> **note** les modules seront disponible sous forme de bibliothèques
statiques et/ou dynamiques selon le système auquel ils sont destinés
(.a, .so / .lib, .dll) et susceptible d'être *linkés* à l'aide des outils
"classiques" (comme ld sous unixoide).

> **note** l'importation des fonctionnalités fournies par une bibliothèque
  développée en C ou en C++ / l'exportation des fonctionnalités d'un module
  vers des fichiers d'en-tête C ou C++ n'a pour seul but que de permettre
  l'utilisation des fonctionnalités développées dans les différents langages
  par différents différents langages.

> **Une idée absurde?** Pourrait-on envisager d'importer une bibliothèque
  C++ sous forme de module pour notre langage et d'exporter le module
  correspondant en C ???

### Des sous-modules à "accès restreint"

Nous le vivons au quotidien dés que nous envisageons de "modulariser" nos
projets:

La différence entre ce que nous voulons que chaque module expose aux autres
et les capacités dont il dispose réellement est généralement phénoménale:
ce n'est pas pour rien que le GoF a identifié le patron de conception "Façade"
dés le début des années (19)90!

En allant plus loin, il se peut que certains "détails d'implémentation"
n'aie du sens -- et ne puissent être utilisés -- que dans l'unité de compilation
dans laquelle ils apparaissent.

Voyez par exemple ma biblithèque de signaux et de slots : j'ai regroupé dans
un espace de noms imbriqué ( `Tools::Details` ) l'ensemble des détails
d'implémentation dont l'utlisateur n'a absolument rien à faire, mais qui permet
au fonctionnalités exposées dans l'espace de noms `Tools` de fonctionner.

Et les exemples similaires sont véritablement légion, ce qui démontre un
réel besoin de pouvoir définir des fonctionnalités comme étant à "usage
interne" du module (ou de l'unité de compilation) de manière simple et
efficace.

### Voir aussi
- [La grammaire associée](grammar.md#modules)
