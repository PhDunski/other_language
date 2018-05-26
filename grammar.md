<a id="top"></a>
# La grammaire du langage

Un soin tout particulier devra être apporté  à la définition de la
grammaire, car  c'est elle qui permettra la création des outils adaptés
à l'utilisation du langage.

Dans un  premier temps, je vais décrire séparément les différentes règles
aussi bien de manière textuelle que sous la forme BNF, sans  essayer de
les asssembler.

<a id="modules"></a>
## L'intégration de module

L'intégration des modules nécesite trois règles principales pour peremttre:

- l'[import d'un module ou d'un sous module](#import)
- l'[import d'un module issu d'un autre langage](#import_alien)
- l'[intégration de des fonctionnalités dans un module ou un sous module](#module_integration)
- l'[intégration de certaines fonctionnalité à un ou plusieurs groupe(s)](#groupe_integration)

<a id="import"></a>
### Importer un module développé dans le langage

La grammaire permettant d'importer un module dans un autre langage doit prendre en compte:

- la possibilité d'importer l'ensemble du module et de ses sous modules
- la possibilité de n'importer que les fonctionnalité d'un sous module particulier
- la possibilité de n'importer qu'une fonctionnalité particulière
- la possibilité d'importer les fonctionnalités définie dans un groupe ou dans un sous groupe

#### La grammaire 

Cette grammaire sera développée plus tard.

<a id="import_alien"></a>
### importer un module issu d'un autre langage

La grammaire permettant d'importer les modules issus d'un autre langage devra
- obliger le développeur à indiquer le langage du module : il y a trop de
différence entre une bibiliothèque C et une bibliothèque C++ (sans meme compter
les différences encore pire avec java ou C#) que pour ignorer cette information
- obliger le développeur à indiquer le nom module à importer
- permettre à l'utilisateur de dresser la liste des fichiers à importer

#### La grammaire 

Cette grammaire sera développée plus tard.

<a id="module_integration"></a>
### L'intégration à un module / un sous module

L'intégration de fonctionnalités à un groupe ou un sous groupe nécessitera un peu de gymnastique, car

- le "module de base" dans lequel les fonctionnalités d'une unité de compilation prendront
  place peut aussi bien etre un module qu'un sous module
- une unité de compilation peut définir certaines fonctionnalités prenant place dans un sous-module
  du "module de base", voir, dans un sous-module de ce sous module
- plusieurs "sous-modules" peuvent etre utilisés dans une seule et meme unité de compilation

#### La grammaire 

Cette grammaire sera développée plus tard.

<a id="groupe_integration"></a>
# L'intégration de fonctionnalités au sein de groupes

La grammaire permettant d'intégrer les fonctionnalités développées dans une unité de 
compilation au sein d'un groupedoit etre suffisamment précise que pour permettre:

- à une fonctionnalité particulière d'apparaitre dans différents groupe
- à certaines fonctionnalités de **ne pas apparaitre** au sein d'un groupe, alors que les
  autres le sont
- à un groupe "plus général" de reprendre l'ensemble des fonctionnalités définies
  au sein d'un groupe plus "spécifique".

#### La grammaire 

Cette grammaire sera développée plus tard.

<a id="structured"></a>
## la programmation structurée

La grammaire associée à la programmation structurée devra prendre en compte:

- l'intégration [de mot clés désignant les types primitifs](#primitif)
- l'intégration de [tests Vrai / Faux](#true_false)
- l'intégration de [tests à choix multiple](#test_multiple)
- l'intégration de [boucles Tant que](#while_loop)
- l'itégration de [boucles Jusque](#do_loop)
- l'intégration de [boucles Pour](#for_loop)
- l'intégration de [boucles sur le contenu d'une collection](#collection_loop)
- l'intégration de [boucle sur des intervalles](#interval_loop)
- l'intégration des [alias de type](#alias)
- l'intégration des [énumérations](#enum)
- l'intégration des [agrégats de donnée](#agregate)
- l'intégration de [la notion de dictionnaire](#dico)
- l'intégration des [unions](#union)
- l'intégration des [expressions lamda](#lambda)
- l'intégration de l'[inférence de type](#inference)

<a id="primitif"></a>
### Les types primitifs

description à venir

#### La grammaire 

Cette grammaire sera développée plus tard.

<a id="true_false"></a>
### Les tests vrai faux

description à venir 

#### La grammaire 

Cette grammaire sera développée plus tard.

<a id="test_multiple"></a>
### les tests à choix multiple

description à venir

#### La grammaire 

Cette grammaire sera développée plus tard.

<a id="while_loop"></a>
### Les boucle "Tant que"

description à venir

#### La grammaire 

Cette grammaire sera développée plus tard.

<a id="do_loop"></a>
### les boucle "jusque"

description à venir

#### La grammaire 

Cette grammaire sera développée plus tard.

<a id="for_loop"></a>
### les boucle "pour"

description à venir

#### La grammaire 

Cette grammaire sera développée plus tard.

<a id="collection_loop"></a>
### les boucles sur le contenu d'une collection

description à venir

#### La grammaire 

Cette grammaire sera développée plus tard.

<a id="interval_loop"></a>
### les boucles sur le contenu d'une collection

description à venir

#### La grammaire 

Cette grammaire sera développée plus tard.

<a id="alias"></a>
### Les alias de type

description à venir

#### La grammaire 

Cette grammaire sera développée plus tard.

<a id="enum"></a>
### Les énumérations

description à venir

#### La grammaire 

Cette grammaire sera développée plus tard.

<a id="agregate"></a>
### Les agrégations

description à venir

#### La grammaire 

Cette grammaire sera développée plus tard.

<a id="dico"></a>
### Les dictionnaires

description à venir

#### La grammaire 

Cette grammaire sera développée plus tard.

<a id="unions"></a>
### Les unions

description à venir

#### La grammaire 

Cette grammaire sera développée plus tard.

<a id="lambda"></a>
### Les expressions lambda

description à venir

#### La grammaire 

Cette grammaire sera développée plus tard.


<a id="inference"></a>
### L'inférence de types

description à venir

#### La grammaire 

Cette grammaire sera développée plus tard.
