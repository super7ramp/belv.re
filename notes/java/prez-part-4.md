---
marp: true
---

![bg right:29% 50%](258px-Java_Logo.svg.png)

# **Java**

## Partie IV - Grand-mère sait faire du bon café

### Les tests

---

# Plan

1. Résumé des épisodes précédents
2. JUnit
3. Mockito
4. Cucumber
5. Prenons du recul : la pyramide des tests


---

## 1. Résumé des épisodes précédents

**Dans la partie I**, on a vu des **généralités** sur le langage. C'était touffu !

**Dans la partie II**, on s'est un peu détendu avec de **l'outillage** : création d'un projet via un IDE et outils de build.

**Dans la partie III**, on est revenu vers le langage lui-même avec la **généricité**.

**Dans cette partie**, on va parler des **tests**. Parce qu'on a déjà de quoi écrire **pas mal de code** mais ce serait mieux que ce soit **du code pas mal** 😋

---

## 2.1. JUnit

[**JUnit**](https://junit.org/junit5/) est le framework de test le plus utilisé.

Tu entendras parler peut-être d'[**AssertJ**](https://github.com/assertj/assertj), qui est sympa aussi !

> Quelque soit le framework utilisé, les tests seront toujours au même endroit : dans le dossier `src/test/java` de ton projet !

---

## 2.2. JUnit - Installation

> Si tu travailles sans outil de build pour commencer, laisse cette étape : ton IDE te proposera de rajouter JUnit à ton projet dès qu'il verra un test !

Pour l'installer, il faut le rajouter en dépendance à ton projet. Exemples :

- Avec  Maven : [exemple de `pom.xml`](https://github.com/junit-team/junit5-samples/blob/main/junit5-jupiter-starter-maven/pom.xml)
- Avec  Gradle : [exemple de `build.gradle`](https://github.com/junit-team/junit5-samples/blob/r5.10.2/junit5-jupiter-starter-gradle/build.gradle)

---

## 2.3. JUnit - Mon premier test 🥹


```java
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.assertEquals;

class PikachuTest {

    @Test
    void shout_strong() {
        final var pika = new Pikachu();
        assertEquals("Pika pika !", pika.shout());
    }

}
```

---

## 2.4. JUnit - Mon premier test 🥹 - Anatomie

```java
// Ici ce sont les imports pour pouvoir accéder aux classes de JUnit
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.assertEquals;
//     ^^^^^^
//     on peut importer une méthode seulement avec import static
//     ça évite d'avoir à écrire Assertions.assertEquals plus bas

// Une classe, parce que tout est classe en Java 😄
class PikachuTest { // teste la classe Pikachu

    @Test // Une annotation ! Ça permet à JUnit de découvrir les tests
    void shout_strong() {
        final var pika = new Pikachu();
        assertEquals("Pika pika !", pika.shout());
        //            ^^^^^^^^^^^   ^^^^^^^^^^^^
        //              attendu         réel
    }

}
```
---

## 2.5. JUnit - Mon premier test 🥹 - Run

TODO image IntellIJ

---

## 2.6. JUnit - Exercice

TODO 

---

## 3. Mockito

TODO

---

## 4. Cucumber

TODO

---

## 5. La pyramide des tests

TODO

---

## Fin de la quatrième partie

Des questions ?

---

## Teaser

Dans les parties suivantes, je pense aborder :

- Les `Stream`s qui rajoutent aux collections la possibilité de faire des `filter()`, `map()` ou `reduce()` que tu as sans doute vu en Javascript
- Les exceptions
- Spring ?