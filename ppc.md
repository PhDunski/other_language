<a id="top"></a>
# Le support de la programmation par contrat

La programmation par contrat est un style de programmation qui tend à "passer un contrat" entre le développeur 
d'une fonctionnalité et celui qui l'utilisera.

Ce contrat est particulièrement simple:

> Donnes moi les données que je veux, tu obtiendras le résultat que tu attends

Si, pour une raison ou une autre, la fonctionnalité n'obtient pas les données qu'elle veut ou que 
la fonctionnalité n'est pas en mesure de fournir le résultat attendu, il y a "rupture du contrat"
et tout peut arriver.

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
