<a id="top"></a>
# Le paradigme générique

Le paradigme générique est basé sur une constatation simple:

> il arrive que l'on ne sache pas **quel type** de donnée nous allons manipuler, que nous sachions 
> en revance parfaitement **comment** nous allons le faire.

Nous pourrions dire que c'est la réflexion en termes de services poussée à son comble.

<a id="idea"></a>
## l'idée générale

L'idée générale de ce paradigme est de fournir au compilateur un **modèle** de la fonctionnalité que nous 
voulons développer et de le laisser générer le code adapté au(x) type(s) de donnée(s) réellement utilisé(s),
une fois qu'il dispose de "toutes les informations".

Ce paradigme permet de mettre en place [polymorphisme paramétrique](https://cpp.developpez.com/faq/cpp/?page=Programmation-objet-en-Cplusplus#Qu-est-ce-que-le-polymorphisme-parametrique)

<a id="particularities"></a>
## La particularité

La particularité essentielle de ce paradigme est que l'évaluation des fonctionnalités qui y ont recours s'effectue
lors de la **compilation**.

<a id="possibilities"></a>
## Les possiblilités

Le paradigme générique être utilisé

- sur des fonctions libres
- pour définir des modèles d'agrégats de données ayant sémantique d'entité dérivé d'une classe de base
- pour définir des modèles d'agrégats de données ayant sémantique de valeur
- pour créer la notion de concept

En outre, il autorise le recours une technique particulière appelée le CRTP

<a id="CRTP"></p>
## Le CRTP

Le CRTP est une technique utilisable grâce au paradigme générique qui permet à un agrégat 
de donnée d'hériter (publiquement) d'un modèle d'agrégat, sous une forme qui pourrait être proche de
```cpp
template <typename Base>
struct equality_omparable{
    bool operator == (Base const & a, Base const &b);
};
template <typename Base>
struct difference_comparable : equality_comparable<Base>{
    bool operator < (Base const & a, Base const &b);
    bool operator > (Base const & a, Base const &b);
    bool operator <= (Base const & a, Base const &b);
    bool operator >= (Base const & a, Base const &b);
}

class time_point : public difference_comparable<time_point>{
    /* ... */
};
```
qui aurait, selon l'exemple, pour résultat de faire en sorte que la classe `time_point` expose l'ensemble des opérateurs de comparaison "classiques" ( `==`, `!=`, `<`, `<=`, `>` et `>=`).

Cette notion est, entre autres parfaite pour aider à  la définition de [concepts](concepts.md)
