<a id="top"></a>
<a id="structured" ></a>
# la programmation structurée

Le langage devra bien entendu supporter l'ensemble des concepts de la
programmation structurée, c'est à dire qu'il devra supporter:

- les [types primitifs](#primitives)
- les [structures de contrôle](#controls)
- les [types définis par l'utilisateurs](#user_defined)
- la [récursivité](#recursive)
- les lambda expressions
- un moyen de [briser le flux normal d'exécution](#exceptions)

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

### les boucles "Jusque"

Une boucle "Jusque" est une boucle pour laquelle la condition qui détermine s'il faut entrer dans la boucle 
est évaluée **après** que le contenu de la boucle ait été exécuté.

### Les boucles "Pour"

Une boucle "Pour" est une boucle dont le nombre d'exécutions est évalué sur base d'un "compteur" ou de tout 
autre moyen d'évaluer le nombre d'exécutions qui ont déjà eu lieu.

Le langage devrait supporter plusieurs sortes de boucles pour :

#### Les boucles Pour "classiques"

Il s'agit de boucle "Pour" pour lesquelles le développeur fournit une valeur de départ, une valeur 
d'arrivée sous forme de condition et un pas permettant de faire évoluer la valeur.

#### Les boucles le contenu d'une collection

Il arrive très régulièrement que nous souhaitions parcourir l'ensemble des éléments d'une collection.  
Le langage devrait permettre au développeur de définir une boucle parcourant ces éléments en ne donnant 
que l'identifiant d'une donnée représentant l'élément "en cours de traitement" et la collection dont il est tiré

### Les boucles basées sur des intervalles
