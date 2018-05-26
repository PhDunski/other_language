<a id="top"></a>
<a id="structured" ></a>
# la programmation structurée

Le langage devra bien entendu supporter l'ensemble des concepts de la
programmation structurée, c'est à dire qu'il devra supporter:

- les [types primitifs](#primitives)
- les [structures de contrôle](#controls)
- les [types définis par l'utilisateurs](#user_defined)
- la [récursivité](#recursive)
- les ["expressions lambda](#lambda)
- l'[inference de type](#inference)
- Un moyen de  [briser le flux d'exécution](execution_stream.md#breaking_stream)

>> **note** Je ne suis pas favorable du tout à l'utilisation de mots clés
   semblable au `continue` et `break` du C++ à l'intérieur de boucles ou
   de tests'

>> **note** Je suis farouchement opposé à la possibilité de créer des sauts
   dans le code.  Tout mot clé similaire au `goto` du C++ est **banni du
   langage**

<a id="primitives></a>
## Les type primitifs

On désigne sous le terme de types primitif les types de données qui sont directement compréhensibles par le langage -- au travers de **mots clé** -- sans qu'il n'ait besoin de quoi que ce soit pour savoir comment les interpréter.

Le langage à développer devrait disposer de type primitifs permettant de représenter

- l'absence de valeur (équivalent de `void` en C et C++)
- des données ne pouvant prendre que les valeur "Vrai" et "Faux" (équivalent au `bool` en C++)
- des valeurs numériques susceptibles de tenir dans l'intervalle autorisé par le nombre de bits contenu par un *byte*, dans une version signée et dans une version non signée (équivalant de `char` dans interprétation en tant que valeur numérique en C et en C++)
- des valeurs numériques susceptibles de tenir dans l'intervall autorisé par un nombre de bits équivalent à deux fois la taille d'un *byte*, dans leur version signée et dans leur version non signée  (équivalant de `short` en C et en C++)
- des valeurs numériques susceptibles de tenir dans l'intervall autorisé par un nombre de bits équivalent à quatre fois la taille d'un *byte*, dans leur version signée et dans leur version non signée  (équivalant de `int` en C et en C++)
- des valeurs numériques susceptibles de tenir dans l'intervall autorisé par un nombre de bits équivalent à huit fois la taille d'un *byte*, dans leur version signée et dans leur version non signée  (équivalant de `long long` en C et en C++)
- l'états des différents bits qui composent un *byte* sans qu'il ne soit question de les interpréter (l'équivalent du `std::byte` apparu en C++17)
- n'importe quel caractère ("glyphe") présent dans la table d'association ASCII (l'équivalent du `char` dans son interprétation sous forme de caractère)
- n'importe quel caractère ("glyphe") présent dans les tables d'association Unicode et ses trois représentations possibles (UTF-8, UTF-16 et UTF-32)
- un réel (nombre "à virgule flottante") proposant la **simple précision** (32 bits)au regard de la norme IEEE 754
- un réel (nombre "à virgule flottante") proposant la **double précision** (64 bits) au regard de la notme IEEE 754
- un réel (nombre "à virgule flottante") proposant la **double précision étendue** (79 bits +1) au regard de la norme IEEE 754

### Voir aussi

- [la grammaire associée](grammar.md#primitif)
<a id="controls"></a>
##les structures de contrôle

Les **structures de contrôle** ont pour objectif de représenter les "déviations normales" du flux d'exécution d'un algorithme et regroupent sous un même terme les moyens qui permettent, dans un langage de programmation de représenter les différents types de **tests** et les différents types de **boucles**.

<a id="tests"></a>
### Les tests

Le langage devra supporter les deux types classiques de tests, à savoir :

### Les tests "vrais / faux"

Les tests "vrai / faux" permettent à une algorithme de choisir un chemin d'exécution sur base de 
l'interprétation d'une expression dont le résultat **doit** être réduit à "Oui" ("Vrai") ou "Non" ("Faux").

Si le résultat de cette évaluation est réduit à "Non" ("Faux") le langage doit permettre au développeur 
d'indiquer un chemin d'exécution alternatif (équivalent du `else` en C++)

#### voir aussi

- [la grammaire associée](grammar.md#true_false)

### Les tests "à choix multiples"

Les tests "à choix multiples" permettent à un algorithme de sélectionner un chemin d'exécution en fonction 
de la valeur d'une donnée particulière.

Dans le cadre de l'intégration de la [programmation par contrat](ppc.md), le fait que la donnée présente 
une valeur qui n'a pas été envisagée serait par **défaut** considérée comme une violation des 
[invariants](ppc.md#invariants) de l'algorithme.

> **note** Dans quelques cas bien particuliers, une valeur non envisagée pour une donnée peut être considérée 
comme une situtation parfaitement légitime par l'algorithme.  Dans ce cas, le langage doit autoriser le développeur
à fournir un chemin d'exécution qui sera choisi "par défaut".

> **note** Si le développeur décide de fournir un chemin d'exécution "par défaut" à un algorithme, cela doit désactiver
  de manière automatique les vérification d'invariants sur la donnée testée.
  

#### voir aussi

- [la grammaire associée](grammar.md#test_multiple)

<a id="loops"></a>
## les boucles

Les boucles permettent d'exécuter plusieurs fois une portion particulière du chemin d'exécution d'un algorithmes.  Le langage devrait supporter les types de boucles suivants:

- les boucles "Tant Que"
- les boucles "Jusque"
- les boucles "Pour"

> **Note** La présence d'une boucle imposant de tester une (ou plusieurs) données de manière régulière, 
> la valeur testée devra **impérativement** être explicitement désignée comme [mutable](changes.md#mutable) 
> sous peine de violer les [invariants](ppc.md#invariants) relatif à cette donnée.
>
> Une donnée constante aurait pour effet d'occasionner, au mieux, une boucle n'ayant aucune 
> raison d'être, car n'étant jamais exécutée, au pire, une boucle infinie, car la valeur de la (des)
> donnée(s) testée(s) ne pourrait pas être modifiée

### Les boucles "Tant que"

Une boucle "Tant que" est une boucle pour laquelle la condition qui détermine s'il faut entrer dans la boucle 
est évaluée **avant** que le contenu de la boucle n'ait été exécuté.

#### voir aussi

- [la grammaire associée](grammar.md#while_loop)

### les boucles "Jusque"

Une boucle "Jusque" est une boucle pour laquelle la condition qui détermine s'il faut entrer dans la boucle 
est évaluée **après** que le contenu de la boucle ait été exécuté.

#### voir aussi

- [la grammaire associée](grammar.md#do_loop)
### Les boucles "Pour"

Une boucle "Pour" est une boucle dont le nombre d'exécutions est évalué sur base d'un "compteur" ou de tout 
autre moyen d'évaluer le nombre d'exécutions qui ont déjà eu lieu.

Le langage devrait supporter plusieurs sortes de boucles pour :

#### Les boucles Pour "classiques"

Il s'agit de boucle "Pour" pour lesquelles le développeur fournit une valeur de départ, une valeur 
d'arrivée sous forme de condition et un pas permettant de faire évoluer la valeur.

#### voir aussi

- [la grammaire associée](grammar.md#for_loop)

#### Les boucles le contenu d'une collection

Il arrive très régulièrement que nous souhaitions parcourir l'ensemble des éléments d'une collection.  
Le langage devrait permettre au développeur de définir une boucle parcourant ces éléments en ne donnant 
que l'identifiant d'une donnée représentant l'élément "en cours de traitement" et la collection dont il est tiré

#### voir aussi

- [la grammaire associée](grammar.md#collection_loop)

#### Les boucles basées sur des intervalles

La possibilité de fournir un intervalle d'exécution peut faciliter énormément la vie du développeur, 
car il suffit en somme de donner à la boucle une valeur de début et une valeur de fin, pour être sûr 
que toutes les valeurs comprises entre les deux soient prises en compte.  Un intervalle peut
être facilement représenté sous la forme d'une expression proche de`[debut .. fin]`, mais d'autres solution
sont envisageable.

#### voir aussi

- [la grammaire associée](grammar.md#interval_loop)

<a id="user_defined"></a>
# Les types définis par l'utilisateur

Un langage qui ne permettrait pas à l'utilisateur de créer ses propres types de données n'aurait qu'un intérêt
très limité.

Pour qu'un langage de programmation "d'usage général" puisse présenter un intérêt réel, il doit permettre au
développeur de définir :

- des [alias de type](#alias)
- des [énumérations et des valeurs énumérées](#enums)
- des [agrégats de données](#agregate)
- des [association clé/valeur et des "dictionnaires"](#assiciate)
- des [unions](#union)

<a id="alias"></a>
## Les alias de type

Comme le dis si bien un héros de film et de livre:

> Le mot, c'est la chose, connait le mot et tu maîtrise la chose.

Le nom que l'on donne à un type de donnée donne une indication très claire de l'usage auquel
la donnée est destinée.

Mais il arrive régulièrement que, dans certaines situations particulières, nous souhaitions
utiliser un type de donnée -- particulièrement adapté à l'usage que nous en ferons -- dont
le nom ne sera pas "suffisemment précis" par rapport à l'usage que nous en ferons.

Il peut alors s'avérer utile et / ou intéressant de donner un autre nom, plus adapté
à l'usage que nous ferons de la donnée à un type de donnée qui existe déjà. Nous voulons
alors pouvoir créer un **alias de type** qui permettra à notre compilateur ou 
à notre interpréteur de reconnaître cet alias exactement comme si nous avions utilisé
le nom d'origine.

### Voir aussi

- [La grammaire associée](grammar#alias)

<a id="enums"></a>
## Les énumérations et les valeur énumérées

Enormément de programmes ont besoin de manipuler des valeurs spécifiques susceptibles de 
représenter des concepts bien particuliers.

Malheureusement, il n'y a rien qui ressemble d'avantage à la valeur `1`qu'une autre valeur `1` 
et l'interprétation que nous pourrions faire de cette valeur s'en retrouve particulièrement 
compliquée.

Pire encore : certains concepts sont souvent représentés sous la forme d'un ensemble de valeurs
bien particulières et nous pouvons régulièrement souhaiter associer un identifiant particulier
à chacune de ces valeurs.  

Prenez l'exemple de la notion de mois: il y a douze mois dans l'année, identifiés par les
noms janvier, février, mars, avril, mai, juin, juillet, aout, septembre, octobre, novembre
et décembre.

Mais nous les associons également à des valeurs particulières en fonction de leur position
dans l'année : janvier est associé à la valeur 1, février à la valeur 2 et ainsi de suite
jusqu'à décembre qui est associé à la valeur 12.

Le plus frustrant de l'histoire, c'est que la plupart des développeurs se foutent pas mal de la valeur 
numérique associée à chaque mois: ce qui les intéresse, c'est de pouvoir identifier clairement
le mois associé à la valeur numérique qu'ils sont occupés à manipuler.

Ce dont le développeur a alors besoin, c'est d'un moyen permettant d'associer **un identifiant** (unique)
à une valeur particulière et de pouvoir regrouper plusieurs identifiants ainsi obtenus autours d'un
concept d'ordre "plus général".

Le concept d'ordre plus général sera désigné sous le terme d'**énumération**, alors que les identifiants
que ce concept rassemble seront  désignés sous le terme de **valeur(s) énumérée(s)**.

### Voir aussi

- [La grammaire associée](grammar#enum)

<a id="agregate"></a>
## Les agrégats de données

Le terme **agregats de données** est un terme parfaitement générique qui désigne la capacité
du développeur à regrouper plusieurs données de type potentiellement différent au sein d'un
type de donnée personnalisé afin d'etre en mesure de les manipuler ensemble.

Ce terme est destiné -- dans le langage que j'aimerais mettre au point -- à etre spécialisé
de manière à différencier les agrégats de données qui présentent une sémantique d'entité et
ceux qui présentent une sémantique de valeur (voir : [différencier les sémantique](changes.md#semantic) ).


<a id="associate"></a>
## Les association clé/valeur et les "dictionnaires"

j'ai toujours regretté que C++ ne fournisse le moyen de créer une association clé / valeur qu'au 
travers d'un élément de la bibliothèque standard.

D'une certaine manière, j'envie les développeurs qui utilisent des langages qui permettent de créer des "dictionnaires" sous une forme proche de

```

auto dico<int, string>{ 1 :> "play", 2 :> "pause" , 3 :> "quit"};

```

Car l'approche du C++ à cet égard est longue et fastidieuse.

### Voir aussi

- [La grammaire associée](grammar#dico)

<a id="union"></a>
## Les unions

Pour etre tout à fait honnete, je n'ai jamais été particulièrement fan de l'utilisation
du concept d'union.

Je dois cependant reconnaitre que le concept en lui-meme offre certaines possibiltiés intéressantes, car 
il consiste à faire en sorte de créer un agrégat de données dont toutes les données partagent le meme 
espace mémoire, ce qui permet d'en inerpréter le contenu de manière différente en fonction des circonstances.

<a id="recursive"></a>
## la récursivité

La récursivité est la capacité d'une fonction à s'appeler elle-meme.

Bien que cette technique augmente considérablement le risque de débordement de la pile d'appels,
et qu'elle provoque finalement un comportement très proche de celui que l'on peut observer 
au travers de boucles, elle permet néanmoins d'exprimer certains algorithme qu'il aurait été
impossible -- ou à tout le moins, particulièrement compliqué -- au travers de boucles "classiques".

> **NOTE** Ce concept de la programmation structurée est l'un des rares -- si pas le seul --
> à ne pas se traduire par une règle grammatical, mais bien par une règle de cohérence lors
> de l'évaluation du résultat.

<a id="lambda"></a>
## Les expressions lambda

Les **expressions lambda** représente la possibilité qu'offre le langage au développeur de définir
ce que l'on peut désigner comme une **fonction anonyme** : un élément de code qui présente toutes 
les caractéristiques d'une fonction (avec valeur de retour, acquisition de paramètres et chemin 
d'exécution) à l'exception d'un identifiant spécifique.

Le concept d'**expression lambda** n'est apparu qu'en C++11), mais je serais désormais bien malheureux 
si je devais m'en passer.

### Voir aussi

- [La grammaire associée](grammar#lambda)

<a id="inference"></a>
## l'inférence de type

L'**inférence de type** est la capacité dont dispose le langage à déterminer par lui-meme le type réel d'une 
expression sur base des informations dont il dispose à ce sujet.

L'inférence de type n'est apparue qu'en C++11, et je ne l'utilise -- pour etre tout à fait honnete -- sans
doute pas au maximum de son potentiel, mais je serais désormais bien malheureux si je devais m'en passer.

### Voir aussi

- [La grammaire associée](grammar#inference)
