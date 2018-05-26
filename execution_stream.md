<a id="top></id>
# Les flux d'exécution

le **flux d'exécution** d'un programme ou d'un algorithme représente l'ensemble des instructions
que ce programme (ou cet algorithme) devra effectuer entre le moment où il commence à s'exécuter
et le moment où son exécution s'acheve.

Dans certaines circonstances, il peut s'avérer opportun d'apporter une "fin prématurée" à ce flux
d'exécution:

- parce que l'algorithme a atteint son objectif "plus tot que prévu"
- parce que l'algorithme est dans l'incapacité de fournir le résultat attendu

Le première situation représente un "cas d'exécution" tout à fait valide que l'on rencontre régulièrement 
lorsque l'on travaille --par exemple-- avec des boucles ou des fonctions récursives

Par contre, la deuxième situation ne survient que lorsqu'une erreur survient lors de l'exécution.

## Quand une erreur survient

Lorsqu'une erreur survient durant l'exécution d'un programme ou d'un algorithme, nous pouvons 
en déduire les deux causes majeures:

- soit le développeur (ou l'utilisateur) de l'algorithme a fait une erreur de logique
- soit l'erreur survient pour une raison indépendante de la volonté du développeur 
  (ou de l'utilisateur) de l'algorithme.

Dans tous les cas, les [règles de la programmation par contrat](ppc.md#rules) sont claires:
l'exécution de l'algorithme doit s'achever **immédiatement**.

Cependant, la manière dont cette exécution s'achève dépend de l'origine de l'erreur, et prend
donc la forme
- d'une [assertion](#assert) en cas d'erreur de logique de la part d'un développeur (ou de l'utilisateur)
- d'une [exception](#exception) en cas d'erreur indépendante de la volonté du développeur (ou de l'utilisateur)

<a id="assert"></a>
## Les assertions

Les **assertion** sont des vérifications effectuées en période de **développement** (à la compilation 
et / ou à l'exécution, selon les circonstances) pour garantir que la logique du **développeur** (ou de
l'utilisateur d'une fonctionnalité) est correcte.

Si une erreur de logique est constatée, le développeur **devra** y apporter une solution **avant** de pouvoir
mettre son travail en production.

Comme les erreurs désignées seront (théoriquement) corrigées, les vérifications ayant permi de les détecter
deviennent parfaitement inutiles en **production** et peuvent dés lors etre absente du code exécutable ayant
atteint cet étape.

<a id="exception"></a>
## Les exceptions

Lorsqu'un algorithme rencontre une erreur indépendante de la volonté de son développeur, il n'est **jamais**
de sa responsabilité d'essayer d'y apporter une solution.

La meilleure chose qu'il puisse alors faire est d'imposer une fin prématurée à son flux d'exéctuion et de
faire "remonter le problème" (au travers de la pile d'appels) dans l'espoir que l'utilisateur (de l'algorithme)
soit en mesure de prendre les mesures nécessaires afin d'y apporter une solution.

Si aucune solution n'a pu etre apportée au problème sur l'ensemble de la pile d'appels, l'exécution du programme
**doit** s'achever sur un signal d'erreur transmis au système d'exploitation.

Ce signal **devrait** auparavent etre transmis à l'utilisateur du programme sous une forme intelligible lui permettant
d'en aviser le développeur.

**Dans l'idéal** l'utilisateur de l'algorithme veillera à mettre au point une procédure susceptible d'apporter une
solution cohérente au problème rencontré.

<a id="approach"></a>
## Différentes approches

L'étude des différents langages qui proposent le support des exceptions démontre que différentes approches sont possibles.

Toutes ces approches ont néanmoins un point commun: la présence d'un bloc d'instructions enrobant les instructions susceptibles 
de lancer une exception et d'un bloc d'instruction enrobant les actions à entreprendre lorsqu'un exception est interceptée
sous une forme proche de

```cpp
try{
    // instructions risquant de lancer une exception
}
catch(/*...*/){
    // instruction à entreprende
}
```

Bien que cette approche soie plus ou moins généralisée, je ne suis pas sur du tout que ce soit
la meilleure manière de procéder.

Je vois en effet quelques "problèmes majeurs" à cette approche dans le sens où:
- elle implique que le dévveloppeur ait bel et bien conscience du fait que certaines instructions 
  qu'il utilise **risquent** de lancer une exception
- elle implique de faire cohatiber le code de traitement de l'erreur (qui est -- rappelons-le --
  en dehors du flux d'exécution "normal") avec le code propre au flux d'exécution normal.
- la logique imposée par la "réintégration" du flux normal lorsqu'une exception a été traitée
  de manière suffisante s'en trouve particulièrement compliquée à mettre en oeuvre.
  
Je préférerais sans doute que le traitement de l'erreur -- lorsqu'il est possible -- se fasse
encore en "dehors" du (mais en relation avec le) flux normal, et que la réintégration ne se fasse
**que** si le traitement envisagé a été en mesure d'apporter une solution.

Cette approche **pourrait**, dans certaines situations, permettre à un algorithme d'essayer
d'apporter une solutions aux problèmes qu'il est en mesure de régler "par lui-meme".

Voici quelques exemples "simples" pour s'en convaincre:


```cpp
void waitOrRetrow(ConnectionError){
    /* le serveur est inconnu, c'est à l'appelant
     * de corriger le problème
     */
    if(error.erroCode() != serverDown)
        throw(e);
    /* on a déjà fais trois essais ou plus...
     * il est temps de "rendre la main"
     */
    if(error.data().tries() >2)
        throw(e);
    /* on attend "un peu" que le
     * serveur se réactive
     */
    std::this_thread::sleep_for(XXXs);
    /* on réintègre le flux d'exécution normale
     * au début de la fonction sendRequest
     */
    
}
void sendRequest(Connection & conn) r
    THREATEXCEPTON<ConnectionError>( waitOrRetrow)// syntaxe précise à défini
{
    /* la connexion avec le serveur est impossible */
    if(! conn.connected)
        throw(ConnectionError(conn));
    /* ... */
}
```
Dans ce premier exemple, le fait que le serveur semble avoir cessé de répondre pourrait parfaitement
etre pris en compte au niveau de la connexion elle-meme et sa réaction face à cette situation
serait -- de toute évidence --systématiquement la meme, peu importe le moment où le problème
est constaté (attendre un certain temps, pour autant que le nombre d'essais autorisés n'ait pas
été atteint, dans le cas présent).

Le seul vrai problème auquel serait confronté le développeur de la connexion serait alors de déterminer
l'endroit où il faut (tenter de) rejoindre le flux d'exécution après "traitement".  Mais cette
information serait fournie par l'utilisateur de la connexion au travers de `ThreatException`, qui correspondrait
littéralement à "si une exception survient traite la en faisant appel à (une fonction particulière) et, 
si le traitement réussi, rejoins le flux d'exécution au début de la fonction `sendRequest`.

un autre exemple, relativement classique serait proche de

```cpp
void clearInput(InputError &){
   std::cout<<"!!!please, stay focused!!!\n";
   /* réinitialise l'entrée standard, après
    * l'échec d'une extraction
    */
   std::cin.clear();
   std::cin.ignore( std::numeric_limits<std::streamsize>::max(), '\n' ); 
    /* on réintègre le flux d'exécution normale
     * au début de la fonction askChoice
     */
}

int askChoice(int max)
    THREATEXCEPTON<InputError>(clearInput)
{
    int choice{0}
    do{
        printMenu(); // affiche un menu avec les possibilités
        std::cin>>choice;
        if(choice==0){
            throw InputError;
        }
    } while(choice < max);
    return choice
}
```
Je reconnais que cet exemple de l'utilisation des exceptions n'est très certainement 
pas le meilleur que l'on puisse donner, mais il est suffisant et clair dans les intentions
de l'utilisateur, qui sont :
- de signaler au compilateur que l'utilisateur s'attend à devoir traiter une exception 
- qu'il s'attend à ce que, si le traitement réussi (ce qu'il ne manquera pas de faire
  dans l'état présent), le compilateur le compilateur organise la réintégration du
  flux d'exécution normal au début de la fonction.
