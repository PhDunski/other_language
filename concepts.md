<a id="top"></a>
# Les concepts

A l'heure actuelle, C++ ne fournit la fonction de **concepts**. 

La notion de concept permet de définir notion qui sera utilisée comme "élément du programme" qui doit respecter certaines
obligations afin d'être utilisable, comme
- la présence de certaines fonctions
- la présence de certaines données

## Mise en oeuvre

J'aimerais que le langage que je m'apprête à  développer intègre la notion de concepts sous une forme qui permettrait
au développeur de définir ses propres concepts sous une forme proches de

```cpp
/* syntaxe à définir */
concept EqualityComparable{
   FREE_OPERATOR == ; 
   FREE_OPERATOR operator != : ()[auto a, auto b]{return ! (a==b);}; // implémentation par défaut
   /* OU - OU - OU */
   REQUIRED bool equal(other); // une fonction membre dédiée
};
concept LessComparable : EqualityComparable{
   
   FREE_OPERATOR operator <= : ()[auto a, auto b]{return a<b || a==b;}; // implémentation par défaut
   FREE_OPERATOR operator < ;
   /* OU - OU - OU */
   REQUIRED bool less(other);       // une fonction membre dédiée
   REQUIRED bool less_equal(other) : [&](auto other){ 
       return a.less(other) || a.equal(other);
   }; // implémentation par défaut
};
concept MoreComparable : EqualityComparable{
   
   FREE_OPERATOR operator <= : ()[auto a, auto b]{return a<b || a==b;}; // implémentation par défaut
   FREE_OPERATOR operator < ;
   /* OU - OU - OU */
   REQUIRED bool more(other);       // une fonction membre dédiée
   REQUIRED bool more_equal(other) : [&](auto other){ 
       return !a.less_equal(other);
   }; // implémentation par défaut
};

concept CompletlyComparable : LessComparable,MoreComparable {
};
```
qui auraient pus être utilisés sous une forme proche de
```
value time_point:
    INVARIANT CompletlyComparable{
/* ... */    
};
```
et qui permette au compilateur de se rendre compte que qu'il **tous** les opérateurs de 
comparaisons **doivent** exister pour l'agrégat identifié par time_point ou que cet agrégat de donnée 
**doit** exposer les fonctions membre `equal`, `less`, `less_equal`, `more` et `more_equals` prenant
toutes une unique donnée en paramètre.

Bien sur, si une implémentation par défaut est fournie, le compilateur devrait:
- fournir la fonction (ou l'opérateur) en question
- autoriser la (re)définition de la fonction (ou de l'opérateur) en cas de besoin
