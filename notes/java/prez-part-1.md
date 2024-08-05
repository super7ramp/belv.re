---
marp: true
---

![bg right:40% 50%](258px-Java_Logo.svg.png)

# **Java**

## Partie I - Apprendre à aimer le café

### Généralités sur le langage

---

# Plan

1. Java en quelques mots
2. Installation
3. Découvrons le langage
   3.1. Les mots-clés
   3.2. Les types primitifs
   3.3. Les opérations
   3.4. Les tableaux
   3.5. Le contrôle de flux
   3.6. Les classes et les objets

---

## 1. Java en quelques mots

- Créé **en 1995** par Sun Microsystems ☀️
- Un langage **orienté objet** 
- Un langage au **typage fort et statique**
- Un langage où on ne gère pas la mémoire à la main (merci le **garbage collector**)
- Un langage compilé dans un langage intermédiaire portable : un **bytecode**
- Un langage dont le bytecode est inteprété par une **machine virtuelle** : la **JVM**
- Un langage utilisé encore assez massivement, en particulier pour le code côté serveurs  !

---

## 2. Installation

![bg right:40% 50%](IntelliJ_IDEA_Icon.svg)

- IntelliJ, un IDE top pour Java : https://www.jetbrains.com/fr-fr/idea/
- Une JDK : https://www.oracle.com/fr/java/technologies/downloads/ (mais passe plutôt par IntelliJ, il permet de télécharger et de gérer plusieurs versions de JDK en 3 clics !)

---

## 3.1. Découvrons le langage - Les mots-clés

Il y a ça :

`boolean byte short int long float double var true false null & && | || = == + += - -= * / ^ % if else switch break do while for class interface enum record extends implements instanceof abstract static private protected public final sealed transient volatile new this import package // /* */ { } [ ] , ; " '`

Et c'est à peu près tout !

---

## 3.2. Découvrons le langage - Les types primitifs

| Nom     | Qu'est-ce que c'est ?                            |
|---------|--------------------------------------------------|
|`boolean`| `true` ou `false`                                |
|`byte`   | un entier sur 8 bit (de -128 à 127)                 |
|`short`  | un entier sur 16 bit (de -32768 à 32767)            |
|`char`   | un caractère sur 16 bit (en soit c'est aussi un entier ! de 0 à 65535) |
|`int`    | un entier sur 32 bit (de -2147483648 à 2147483647) |
|`long`   | un entier sur 64 bit (trop de chiffres 😄 de -2<sup>63</sup> à 2<sup>63</sup>-1)       |
|`float`  | un nombre à virgule sur 32 bit                   |
|`double` | un nombre à virgule sur 64 bit                   |

---

## 3.3. Découvrons le langage - Les opérations - 1

| Nom     | Qu'est-ce que c'est ?                            |
|---------|--------------------------------------------------|
|`=`      | L'assignation                                    |
|`==`     | Le test d'égalité                                |
|`+`      | Tu le sais                                       |
|`-`      | Celui là aussi                                   |
|`*`      | Encore un                                        |
|`/`      | Décidément                                       |
| `%`     | Le modulo                                        |
|`\|\|`   | Le "ou"                                          |
|`&&`     | Le "et"                                          |

Note : à la différence d'autres langages, ces opérateurs ne marchent que pour les types primitifs, à l'exception de `=` et `==` (et du `+`, pour le type `String` que l'on verra plus tard).


---

## 3.3. Découvrons le langage - Les opérations - 2

On a tout ce qu'il faut pour commencer !

Lance un `jshell` et tape :

<style scoped>
pre {
  font-size: 0.6em;
}
</style>

```java
1 + 1       // 2, un int
1.0 + 1.0   // 2.0, un double
1.0 + 1     // 2.0, encore un double
1.0f + 1.0f // 2.0, un float
1.0f + 1.0  // 2.0, un double
```

Donc on a des transformations automatiques de types !

Mais pas pour tout, ce n'est pas du Javascript :

```java
1 + true
//|  Error:
//|  bad operand types for binary operator '+'
//|    first type:  int
//|    second type: boolean
//|  1+true
//|  ^----^
```

---

## 3.3. Découvrons le langage - Les opérations - 3

```java
 // j'assigne la valeur 42 à la variable answer de type int
int answer = 42;

// j'assigne la valeur la variable answer à la variable life - la valeur est copiée
int life = answer;
answer = 43;

// life vaut toujours 42 !
```

---

## 3.4. Découvrons le langage - Les tableaux

Un tableau, c'est une zone contigue en mémoire de variables d'un même type. Oui.

On utilise le mot clé `new` pour le créer

> Le `new`, c'est parce qu'on alloue de la mémoire sur le tas (la `heap`). Jusqu'à présent on travaillait avec les types primitifs sur la pile (la `stack`).

```java
int[] myArray = new int[3]; // {0,0,0}
myArray.length; // 3
myArray[0] = 3; // {3,0,0}
myArray[1] = 4; // {3,4,0}
myArray[2] = 5; // {3,4,5}

int[] myOtherArray = new int[] { 3, 4, 5 }; // {3,4,5}
```

---

## 3.5. Le contrôle de flux - Branchements - 1

Comme Javascript !

### if-else

```java
boolean condition = true;
int value;
if (condition) {
    value = 1;
} else {
    value = 0;
}
```

---

## 3.5. Le contrôle de flux - Branchements - 2

### switch

Juste pour l'exemple, les `switch` en vrai c'est pas hyper utile.

<style scoped>
pre {
  font-size: 0.7em;
}
</style>

```java
int input = 0;
int output;
switch(input) { // Le classique
    case 1:
    case 2:
        output = 12;
        break;
    case 3:
        output = 3;
        break;
    default: output = 4;
};
output = switch(input) { // Le moderne
    case 1, 2 -> output = 12;
    case 3 -> output = 3;
    default -> output = 4;
};
```

---

## 3.5. Le contrôle de flux - Les boucles - for

La boucle `for` classique :

```java
int[] myArray = new int[] { 3,4,5 };
int sum = 0;
for (int i = 0; i < array.length; i++) {
    sum += myArray[i];
}
```

La boucle `for` améliorée :

```java
int[] myArray = new int[] { 3,4,5 };
int sum = 0;
for (int element : myArray) {
    sum += element;
}
```

---

## 3.5. Le contrôle de flux - Les boucles - while

```java
int[] myArray = new int[] { 3,4,5 };
int sum = 0;
int i = 0;
while (i < myArray.length) {
    sum += myArray[i];
    i++;
}
```

---

## 3.6. Les classes et les objets - Qu'est que c'est ? - 1

Un truc fondamental dans Java, ce sont les **objets**.

Un objet c'est :

- Une structure de données
- Des **méthodes** (des fonctions) permettant de manipuler les données

Prenons l'exemple du type (ou *classe*) `String`.

```java
String beetlejuice = "Beetlejuice";
beetlejuice.isEmpty(); // false
beetlejuice.length(); // 11
beetlejuice.charAt(0); // 'B'
beetlejuice.repeat(3); // "BeetlejuiceBeetlejuiceBeetlejuice"
// and a lot more methods
```
---


## 3.6. Les classes et les objets - Qu'est que c'est ? - 2

Dans l'exemple précédent, `beetlejuice` est un objet de type `String`. On dit aussi que c'est **une instance** de la classe `String`.

Voici à quoi ressemble cette classe (son nom complet est `java.lang.String`):

```java
public final class String
    implements java.io.Serializable, Comparable<String>, CharSequence, Constable, ConstantDesc {

    private final byte[] value;
    // ...
    @Override
    public boolean isEmpty() {
        return value.length == 0;
    }
    // ...
```

(En vrai la classe est énorme, elle fait presque 5000 lignes !)

---

## 3.6. Les classes et les objets - Créer un objet - 1

La classe `String` est un peu particulière, dans le sens où pour créer un objet de type `String`, il suffit de mettre des `"` autour de caractères.

Pour les classes "normales", on utilise l'opérateur `new` (oui, comme pour les tableaux... en fait, j'ai menti : les tableaux sont des objets 😋) :

```java
class Pikachu {
    int lifeInPercent;
    Pikachu() {
        lifeInPercent = 100;
    }
}
Pikachu sachasPikachu = new Pikachu();
```

---

## 3.6. Les classes et les objets - Créer un objet - 2

Il est possible de passer des paramètres au constructeur pour customiser la création de l'objet :

```java
class Pikachu {
    int lifeInPercent;
    Pikachu(int lifeInPercent) {
        this.lifeInPercent = lifeInPercent;
    }
}
Pikachu wildPikachu = new Pikachu(75);
```

---

## 3.6. Les classes et les objets - Créer un objet - 3

Il est possible d'avoir plusieurs constructeurs

```java
class Pikachu {
    int lifeInPercent;
    Pikachu() {
        this(100);
    }
    Pikachu(int lifeInPercent) {
        this.lifeInPercent = lifeInPercent;
    }
}
Pikachu wildPikachu = new Pikachu(75);
```

---

## 3.6. Les classes et les objets - Méthodes

Et surtout il est possible de rajouter des méthodes à une classe d'objets !

```java
class Pikachu {
    int lifeInPercent;
    Pikachu() {
        this(100);
    }
    Pikachu(int lifeInPercent) {
        this.lifeInPercent = lifeInPercent;
    }
    int lifeInPercent() {
        return lifeInPercent;
    }
    String shout() {
        if (lifeInPercent() > 75)
            return "Pika Pika !";
        if (lifeInPercent() > 50)
            return "Pika !";
        if (lifeInPercent() > 0)
            return "Pikaaa...";
        return "";
    }
}
Pikachu wildPikachu = new Pikachu();
wildPikachu.lifeInPercent(); // 100
wildPikachu.shout(); // "Pika Pika !"
```

---

## 3.6. Les classes et les objets - Encapsulation de données - 1 

Une idée importante derrière les objets, c'est **l'encapsulation des données**. Cela veut dire que dans un objet on cherche à exposer le moins possible les données. On va plutôt fournir des méthodes pour travailler sur ces données.

Comme ça, si la structure de données évolue, le code appelant n'a pas à changer, seule l'implémentation de la méthode change.


<div style="display: flex;
            justify-content: center;
            align-items: baseline">
    <span style="font-size: 196px">🪆</span>
    <span style="font-size: 128px">🪆</span>
    <span style="font-size: 96px">🪆</span>
    <span style="font-size: 64px">🪆</span>
    <span style="font-size: 48px">🪆</span>
</div>

---

## 3.6. Les classes et les objets - Encapsulation de données - 2 

```java
class Pikachu {
    float life; // On change le type de la donnée
    Pikachu() {
        this(100);
    }
    Pikachu(int lifeInPercent) {
        this.life = ((float) lifeInPercent) / 100; // ce code change
    }
    int lifeInPercent() {
        return (int) (100 * life); // ce code change
    }
    String shout() {
        // Le code pourrait changer là aussi mais même pas
        if (lifeInPercent() > 75)
            return "Pika Pika !";
        if (lifeInPercent() > 50)
            return "Pika !";
        if (lifeInPercent() > 0)
            return "Pikaaa...";
        return "";
    }
}
Pikachu wildPikachu = new Pikachu();
wildPikachu.lifeInPercent(); // Et surtout le code ne change pas côté appelant !
```

---

## 3.6. Les classes et les objets - Visibilité

Toujours dans la même optique d'encapsulation des données, Java a une gestion assez fine de l'accessibilité des champs et méthodes d'une classe d'objets. On parle de **visibilité**.

```java
public class Pikachu { // public = la classe est visible depuis toutes les autres classes
    private float life; // private = le champ n'est pas accessible depuis une autre classe
    public Pikachu() { // public = un objet peut être instancié depuis toutes les autres classes
        this(100);
    }
    Pikachu(int lifeInPercent) { // visibilité par défaut = un objet peut être instancié depuis les classes du même package
        this.life = ((float) lifeInPercent) / 100;
    }
    public int lifeInPercent() { // public = la méthode est visible depuis toutes les autres classes
        return (int) (100 * life);
    }
    public String shout() { // public = la méthode est visible depuis toutes les autres classes
        // ...
    }
}
```

---

## 3.6. Les classes et les objets - L'héritage - `extends`

Un autre but important de l'orientation objet, c'est la **réutilisation de code**.

**L'héritage** est un bon moyen pour centraliser un comportement partagé par plusieurs classes dans une classe **parente**.

<style scoped>
pre {
  font-size: 0.5em;
}
</style>
```java
public abstract class PokemonBase {
    private float life;
    public PokemonBase(int lifeInPercent) {
        this.life = ((float) lifeInPercent) / 100;
    }
    public int lifeInPercent() {
        return (int) (100 * life);
    }
}
public class Pikachu extends PokemonBase { // classe enfante
    // life est déclaré dans la classe parente
    public Pikachu() {
        this(100);
    }
    Pikachu(int lifeInPercent) {
        super(lifeInPercent); // super, c'est le this de la classe parente
    }
    // lifeInPercent() est fourni par la classe parente
    public String shout() {
        // ...
    }
}
```

---

## 3.6. Les classes et les objets - L'héritage - Surcharge

Une classe enfante peut **surcharger** (*override*) l'implémentation d'une classe parente.

```java
public abstract class PokemonBase { // classe parente
    protected float life; // protected rend visible le champ aux classes enfantes
    // ...
    public final int lifeInPercent() {
        return (int) (100 * life);
    }
}
public class Pikachu extends PokemonBase { // classe enfante
    // ...
    @Override
    public int lifeInPercent() {
        if (life > 0.42) {
            return 100;
        }
        return super.lifeInPercent(); // on appelle l'implémentation de la classe parente
    }
}
```

---

## 3.6. Les classes et les objets - L'héritage - `final`

Le mot-clé `final` permet d'empêcher une méthode d'être **surchargée** (*overridden*) par une classe enfante :

<style scoped>
pre {
  font-size: 0.6em;
}
</style>
```java
public abstract class PokemonBase { // classe parente
    // ...
    public final int lifeInPercent() {
        return (int) (100 * life);
    }
}
public class Pikachu extends PokemonBase { // classe enfante
    // ...
    @Override
    public int lifeInPercent() {
        return 42;
    }
}
// |  Error:
// |  lifeInPercent() in Pikachu cannot override lifeInPercent() in PokemonBase
// |    overridden method is final
// |      @Override
// |      ^--------...

```

---

## 3.6. Les classes et les objets - Interface

Une alternative à la classe abstraite, quand on veut juste déclarer un comportement commun à plusieurs objets sans fournir d'implémentations (sans gérer un état dans une classe parente en fait), c'est **l'interface**.

```java
interface Pokemon() {
    String shout();
}

public abstract class PokemonBase implements Pokemon {
    // ...
}

public class Pikachu extends PokemonBase {
    // ...
    @Override // ça c'est une annotation, c'est optionnel
    public String shout() { // le programme ne compile pas si la méthode n'est pas implémentée ici
        // ...
    }
}
```

---

## 3.6. Les classes et les objets - Exercice 1

1. Récupère/réécris `Pokemon`, `PokemonBase` et `Pikachu`.
2. Crée une classe `Salameche` implémentant `Pokemon`.
3. Crée trois Salamèche et deux Pikachu et fais les crier. Pour ça, mets-les dans un tableau... pardon une pokeball et itère !

---

## 3.6. Les classes et les objets - `static` - 1

On peut mettre dans des classes des méthodes (ou des champs) dont l'implémentation (ou la valeur) ne dépend pas d'une instance particulière. C'est pratique pour les constantes notamment ou les méthodes travaillant sur des constantes.

Pour cela, on utilise le mot-clé `static`.

```java
public abstract class PokemonBase implements Pokemon {
    private static final float MAX_LIFE = 1.0;
    private static final float MIN_LIFE = 0.0;
    private float life;
    // ...
    public static int maxLifeInPercent() {
        return lifeInPercent(MAX_LIFE);
    }
    public static int minLifeInPercent() {
        return lifeInPercent(MIN_LIFE);
    }
    private static int lifeInPercent(final float life) {
        return (int) (100 * life); // life en paramètre, this n'existe pas !
    }
    // Note : on peut avoir le même nom de méthode, pourvu qu'on peut la différencier (paramètres différents)
    public int lifeInPercent() {
        return lifeInPercent(life); // = life de l'objet, this.life
    }
}
```

---

## 3.6. Les classes et les objets - `static` - 2

```java
public class Pikachu extends PokemonBase {
    // ...
}

Pikachu wildPikachu = new Pikachu(Pikachu.maxLifeInPercent());
wildPikachu.lifeInPercent(); // 100
wildPikachu.shout(); // "Pika Pika !"
```

---

## 3.6. Les classes et les objets - L'`enum` - 1

L'`enum` est une forme de classe particulière : elle permet de définir des classes avec un nombre fixe d'implémentations :

```java
enum PokemonType {
    FIRE, // par convention, on met en capitale les implémentations
    WATER,
    ELECTRIC,
}
```

C'est pratique pour grouper des constantes !

---

## 3.6. Les classes et les objets - L'`enum` - 2

On peut faire plus de choses qu'il n'y parait avec un `enum` :

```java
enum PokemonType {
    // Les instances
    FIRE("Fire-type moves are super effective against Bug-, Grass-, Ice-, and Steel-type Pokémon"),
    WATER("Water-type moves are super effective against Fire-, Ground-, and Rock-type Pokémon"),
    ELECTRIC("Electric-type moves are super effective against Flying- and Water-type Pokémon");
    // note le point-virgule ici -------------------------------------------------------------^

    // L'implémentation, comme une classe classique !
    private final description;

    PokemonType(String description) {
        this.description = description;
    }

    String description() {
        return description;
    }
}
```

---

## 3.6. Les classes et les objets - Exercice 2

1. Reprends/réécris le code de `PokemonType`.
2. Ajoute à l'interface `Pokemon` la méthode `type()` qui retourne un objet de type `PokemonType`.
3. Modifie la hiérarchie de classe :
   - Introduis `ElectricPokemon` et `FirePokemon` : c'est elles qui implémenteront `type()`
   - Ça doit faire : `Pikachu` -> `ElectricPokemon` -> `PokemonBase` -> `Pokemon` et `Salameche` -> `FirePokemon` -> `PokemonBase` -> `Pokemon`
4. Afficher les types de Pokemon dans ta pokeball !

---

## 3.6. Les classes et les objets - Allons plus loin - Object

Tous les objets étendent implicitement la classe `Object` (dans `java.lang`). On aura l'occasion de revenir sur cette classe !

```java
/**
 * Class {@code Object} is the root of the class hierarchy.
 * Every class has {@code Object} as a superclass. All objects,
 * including arrays, implement the methods of this class.
 *
 * @see     java.lang.Class
 * @since   1.0
 */
public class Object {
    public Object() {}
    // ... et 500 lignes de plus tout de même !
}
```

---

## 3.6. Les classes et les objets - Allons plus loin - Hello World ?

Le **Hello world!** à la fin ? Oui, je suis comme ça 😋

Le **Hello world!** de Java est souvent présenté comme lourdingue. En effet, il s'écrit :

```java
System.out.println("Hello World!")
```

Qu'est-ce que fait cette ligne ? Essaie d'expliquer chaque élément !

`jshell` offre la complétion, si tu fais `<tab><tab>` sur chaque partie, tu as la possibilité d'accéder à la doc !

(Réponse slide suivante.)

---

## 3.6. Les classes et les objets - Allons plus loin - Hello World !

D'abord, on voit `System`. C'est le nom d'une classe (toutes les classes commencent par une majuscule par convention... je ne te l'avais pas dit ? 😄).

`out` est un champ (`static`) de la classe. Il est de type `PrintStream`, qui est une classe qui définit un flux sortant. En gros, quelque chose sur lequel on peut écrire.

`println()` est une méthode définie par `PrintStream` qui écrit la chaine de caractères passée en paramètre sur ce flux sortant, en rajoutant un caractère de fin de ligne. 

C'est un peu lourd ! C'est une conséquence de la conception de Java centrée autout des classes, et de l'absence de "sucre" (😋) pour alléger la syntaxe !

---

## Fin de la première partie

Des questions ?

---

## Teaser

Dans les parties suivantes, je pense aborder :

- Les collections (des classes telles que `List`, `Set`, `Map` qui permettent de faire plus de trucs que les simples tableaux)
- Créer un projet dans un IDE (créer des packages, créer des classes dans des fichier `.java`, créer un `main`, compiler et lancer le projet)
- Les `Stream`s qui rajoutent aux collections la possibilité de faire des `filter()`, `map()` ou `reduce()` que tu as sans doute vu en Javascript
- Les tests avec JUnit
- Les exceptions