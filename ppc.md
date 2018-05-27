<a id="top"></a>
# Le support de la programmation par contrat

La programmation par contrat est un style de programmation qui tend à "passer un contrat" entre le développeur 
d'une fonctionnalité et celui qui l'utilisera.

Ce contrat est particulièrement simple:

> Donnes moi les données que je veux, tu obtiendras le résultat que tu attends

Si, pour une raison ou une autre, la fonctionnalité n'obtient pas les données qu'elle veut ou que 
la fonctionnalité n'est pas en mesure de fournir le résultat attendu, il y a "rupture du contrat"
et tout peut arriver.

<a id="notions"></a>
## Trois notions essentielles

La programmation parcontrat a recours à trois notions primaires:

- les [invariants](#invariant)
- les [pré conditions](#precondition)
- les [post conditions](#postcondition)

Ces trois notions ont pour but de garantir la "bonne fin" du contrat passé entre l'algorithme 
et l'utilisateur, ou, si vous préférez, le fait que le résultat obtenu correspondra effectivement 
au résultat souhaité.
<a id="invariant"></a>
### Les invariants
Le terme "invariant" représente les éléments qui doivent etre validés aussi bien en tant que précondition 
qu'en tant que post condition.

**Si** (et **seulement si**) un algorithme peut **garantir** sa "bonne fin" ainsi que le fait qu'il **ne 
sera pas interrompu** (entre autres par un autre thread qui accéderait aux memes donnés), les invariants 
**peuvent** ne pas etre **temporairement** respectés durant l'exécution de l'algorithme,

> **NOTE** Il va de soi que cette autorisation de non respect **temporaire** doit etre utilisée **avec
  parcimonie**

<a id="precondition"></a>
### les pré conditions

Le terme "pré condition" représente les conditions qui **doivent** etre validées **au plus tard**
au moment où l'exécution de l'algorithme **commence**.

Cela inclut entre autres -- mais pas de manière exhaustive -- les invariants.

Si ces conditions ne sont pas validées, cela indique une **erreur de logique** de la part de 
**l'utilisateur** de l'algorithme, qui **devra** les corriger avant le passage en production
de la fonctionnalité.

<a id="postcondition"></a>

le terme "post condition" représente les conditions qui **doivent** etre validées **au plus tard**
au moment où l'algorithme **s'achève**.

Cela inclut entre autres -- mais pas de manière exhaustive -- les invariants.

Deux possibilités sont envisageables en cas de non respect des post conditions:

- l'erreur porte sur une erreur de la part du **développeur** de l'algorithme, comme 
  -- de manière non exhaustive -- un non respect des invariants
- l'erreur porte sur un élément indépendant de la volonté du développeur de l'algorithme

Le premier cas **devra** etre corrigé **avant** que le développeur de l'algorithme ne
le mette en production; le deuxième cas **devrait** etre pris en compte par l'utilisateur
de l'algorithme.


<a id="rules"></a>
## Une règle

Au final, la programmation par contrat n'impose qu'une seule règle : : si un algorithme est 
dans l'incapacité de fournir le résultat attendu par son utilisateur, il doit arreter le traitement
le plus vite possible, pour éviter que la suite du traitement ne corrompe les données.

### deux possiblités

La manière dont cette règle sera appliquée dépendra de la raison pour laquelle l'algorithme est
incapable de fournir le résultat attendu.

- Soit, l'erreur survient suite à une erreur de logique de la part de l'utilisateur de l'algorithme,
  comme une tentative d'accès à un indice hors limites
- soit l'erreur survient suite à un problème "indépendant de la volonté" de l'utilisateur, comme 
  l'inaccessibilité (que l'on espère temporaire) d'un serveur distant.

Dans le premier cas, cette erreur **devra** etre corrigée par l'utilisateur de l'algorithme avant
que l'application ne soit mise en production, et les vérifications effectuées afin de nous prémunir
du problème n'auront plus de raisons d'etre

Dans le deuxième cas, il n'appartient jamais à l'algorithme qui est confronté au problème d'essayer
d'y apporter une solution.  Mais l'utilisateur de l'algorithme serait "bien inspiré" de prévoir une
"procédure de secours" permettant d'y apporter une solution, à l'endroit de son code où il devient
possible de le faire.


<a id="also"></a>
## Voir aussi
- [Buts du projet](README.md#top)
- [Une description succinte](description.md#top)
- [Corrections et avantages](changes.md#top)
- [les fonctionnalités du langage](functionalities.md#top)
- [la modularité](modules.md#top)
- [La grammaire et la syntaxe](grammar.md#top)
