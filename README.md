<a id="top"></a>
# Un autre langage de programmation

Ce n'est un secret pour personne : de tous les langages de programmation
que je connais, ma préférence va clairement au C++.

Cette préférence est essentiellement due  la
philosophie du langage dont on peut aisément tracer les grandes
lignes:

- on ne paye le prix de ce que l'on utilise
- on privilégie l'utile au "sécuritaire"
- on ne se limite pas à un paradigme particulier

Malheureusement les qualités excetpionnelles du C++ ne doivent pas occulter les quelques défauts dont il souffre selon moi.
Parmi ceux-ci, je citerais :

- la constance explicite
- un système de déclaration des fonctionnalités archaïque
- le sens ambigu du mot clé `static`
- la non différentiation des sémantiques
- une intégration imparfaite de la programmation par contrat
- l'acceptation de décisions conceptuellement aberrantes
- une trop grande compatibilité avec C
- l'ordre d'évaluation des arguments et des opérandes est mal défini
- le manque de distinction entre un `char` et un `byte` (avant C++17)
- les exceptions dans la grammaire
- l'inférence de type est trop complexe

Ce projet a pour but dévaluer la faisabilité de créer un nouveau langage qui profiterait de l'expérience acquise
grâce à C++ pour essayer d'en corriger les défauts les plus criants

<a id="why" ></a>
## Pourquoi un nouveau langage?

Vous auriez tout à fait raison de vous demander la raison qui me pousse
à essayer de créer un nouveau langage.

Cette raison est finalement toute simple : malgré les énormes qualités
que je reconnais à C++, je lui trouve quelques défauts qui mériteraient
-- selon moi -- d'être corrigés.

Malheureusement, ces défauts font intrinsèquement partie de son ADN,
si bien qu'il serait difficiles de les corriger.

Parmi les principaux, j'ai envie de citer :


Ces quelques défauts rendent -- à mon humble avis -- C++ bien plus complexe
qu'il n'aurait pu l'être si il n'en avait pas souffert.

Le but de ce projet est (d'essayer) de créer un langage qui aurait toute
la puissance du C++ mais qui en résoudrait les problèmes les plus criants.

<a id="also"></a>
## Voir aussi
- [Description générale](description.md#top)
- [Corrections et avantages](changes.md#top)
- [La modularité](modules.md#top)
- [les fonctionnalités du langage](functionalities.md#top)
- [La grammaire et la syntaxe](grammar.md#top)
