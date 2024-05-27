---
marp: true
---

![bg right:40% 50%](258px-Java_Logo.svg.png)

# **Java**

## Partie II - Ne pas se noyer dans une tasse de café

### Anatomie d'un projet Java & tooling

---

<!-- paginate: true -->

# Plan

1. Résumé de l'épisode I
2. Anatomie d'un projet Java
2.1. Aperçu
2.2. Le `main`
2.3. Projet vs. module vs. package
3. Mon premier projet 🥹
4. Le build
4.1. C'est simple mais compliqué
4.2. Ant, c'était avant
4.3. Maven
4.4. Gradle

---

# 1. Résumé de l'épisode I

Java, c'est :

- Une syntaxe relativement proche du Javascript.
- Un **typage** plus fort que le Javascript.
- Un langage orienté objet
    - Le concept de base est **l'objet**, qui est une instance de la **classe**, et qui **encapsule** des données dans des **champs**.
    - Les fonctions s'appellent **méthodes** et sont portées par les objets.
    - Certaines methodes ou champs sont communs à toutes les instances d'une classe : ils sont marqués avec le mot-clé **`static`**.

---

# 2. Anatomie d'un projet Java - Aperçu

Dans la partie I, on travaillait avec `jshell`. Mais en vrai, on travaille avec des fichiers !

L'arborescence typique d'un projet est la suivante :

```
src
├── main
│   ├── java                       // src/main/java : sources Java
│   │   └── re
│   │       └── belv
│   │           └── pokemon        // re.belv.pokemon : un package
│   │               └── Main.java  // Du code source. Un fichier .java = une classe.
│   └── resources                  // src/main/resources : ressources diverses (configuration par ex.)
└── test
    ├── java                       // src/test/java : tests Java
    └── resources                  // src/test/resources : ressources de test
```

---

# 2. Anatomie d'un projet Java - Le `main`

C'est le point d'entrée de l'application. C'est ce code que la JVM appelle, c'est de là que tout part !

```java
public class Main {
    public static void main(String[] args) {
        System.out.println("Hello World!");
    }
}
```

`args` sont les arguments passés en ligne de commande à l'application.

---

# 2. Anatomie d'un projet Java - Projet vs. package vs. module

En Java :

- Un **package** est un groupe de classes.
- Un **projet** est un groupe de packages : c'est un concept qui ne fait pas partie du langage cela dit, mais des IDE/outils de build.
- Un **module** est aussi un groupe de packages : c'est un concept qui fait partie du langage depuis Java 9, mais il est encore relativement peu utilisé.

> Et c'est là que je me dis qu'on aurait dû voir le concept de visibilité ici, ça aurait été bien mieux que dans la partie I 😅

---

# 3. Mon premier projet 🥹

Crée un nouveau projet avec les classes faites dans la partie I. Lance le dernier exo dans le main !

> Note : l'IDE va te proposer plusieurs outils de build (par exemple IntelliJ propose Maven, Gradle ou rien du tout, juste l'IDE). Peu importe ici ! Quelque soit l'IDE, tu auras un bouton avec une flèche pour exéuter `main` 😄 On voit les différences entre les outils de build juste après 😉

---

# 4.1. Le build - C'est simple mais compliqué

On l'a vu tout au début : le code source Java (fichier `.java`) doit être compilé en bytecode (fichier `.class`) pour être ensuite interprété par la JVM.

En pratique, les `.class` et les ressources sont regroupées dans des archives zippées portant l'extension `.jar`  - ce sont de simples fichiers `.zip` !

Il n'y a souvent pas qu'un seul `jar` dans un projet, mais une foultitude : le(s) tien(s) + toutes les librairies que tu utilises.

C'est le boulot d'un outil de build de compiler le code, les mettre dans des `jar`s et de donner un moyen facile de lancer l'application.

---

# 4.2. Ant, c'était avant - Exemple

![h:24](ant.gif) Ant, c'est le premier (?) système de build conçu pour Java.

La configuration du build s'écrit dans un fichier XML. Exemple :

<style scoped>
pre {
  font-size: 0.3em;
}
</style>

```xml
<project name="MyProject" default="dist" basedir=".">
  <description>
    simple example build file
  </description>
  <!-- set global properties for this build -->
  <property name="src" location="src"/>
  <property name="build" location="build"/>
  <property name="dist" location="dist"/>

  <target name="init">
    <!-- Create the time stamp -->
    <tstamp/>
    <!-- Create the build directory structure used by compile -->
    <mkdir dir="${build}"/>
  </target>

  <target name="compile" depends="init"
        description="compile the source">
    <!-- Compile the Java code from ${src} into ${build} -->
    <javac srcdir="${src}" destdir="${build}"/>
  </target>

  <target name="dist" depends="compile"
        description="generate the distribution">
    <!-- Create the distribution directory -->
    <mkdir dir="${dist}/lib"/>

    <!-- Put everything in ${build} into the MyProject-${DSTAMP}.jar file -->
    <jar jarfile="${dist}/lib/MyProject-${DSTAMP}.jar" basedir="${build}"/>
  </target>

  <target name="clean"
        description="clean up">
    <!-- Delete the ${build} and ${dist} directory trees -->
    <delete dir="${build}"/>
    <delete dir="${dist}"/>
  </target>
</project>
```

---

# 4.2. Ant, c'était avant - Comment faire si on te donne un build Ant ?

Si on te donne un projet géré avec Ant, tu as 3 possibilités :

1. Tu migres le projet vers Maven ou Gradle, pour t'entraîner.
2. Tu confies le projet à l'alternante, en lui donnant [la doc](https://ant.apache.org/manual/index.html), s'il-te-plaît. Ça ne marche pas si c'est toi l'alternante bien sûr 😄
3. Tu fuies 💨

--- 

# 4.3. Maven - Qu'est-ce que c'est ?

![h:24](maven.png) est le successeur de Ant.

L'avantage de Maven, c'est qu'il gère les dépendances : il va télécharger depuis un dépôt les librairies nécessaires pour compiler ton programme. Parce que non, Ant ne le fait pas 😅

Une grande réussite de Maven, c'est d'avoir introduit ou démocratisé des conventions qui sont aujourd'hui partagées par tous les programmes et librairies Java, par exemple :

- La [structure des projets](https://maven.apache.org/guides/introduction/introduction-to-the-standard-directory-layout.html) en `src/{main,test}/{java,resources}`
- Le [nommage des packages et des artefacts](https://maven.apache.org/guides/mini/guide-naming-conventions.html) à partir d'un nom de domaine inversé (RDNS)

> Par *artefact* je veux dire ici, un `jar`, publié sur un dépôt tel que [Maven Central](https://mvnrepository.com/repos/central).

---

# 4.3. Maven - Exemple

Exemple de `pom.xml`, le *Project Object Model*, c'est-à-dire la conf du build pour Maven :

<style scoped>
pre {
  font-size: 0.5em;
}
</style>

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>
 
  <groupId>com.mycompany.app</groupId>
  <artifactId>my-app</artifactId>
  <version>1.0-SNAPSHOT</version>
 
  <properties>
    <maven.compiler.source>1.7</maven.compiler.source>
    <maven.compiler.target>1.7</maven.compiler.target>
  </properties>
 
  <dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.12</version>
      <scope>test</scope>
    </dependency>
  </dependencies>
</project>
```

Pour compiler : `mvn compile`.

Plus d'infos dans [la doc](https://maven.apache.org/guides/getting-started/maven-in-five-minutes.html).

---

# 4.3 Gradle - Qu'est-ce que c'est ?

![h:38](gradle.svg), c'est le dernier système de build sorti pour Java.

La force de Gradle, comparée à Maven, c'est de permettre d'écrire ses fichiers de build dans un langage de programmation "classique" - en [Groovy](https://groovy-lang.org/) ou en [Kotlin](https://kotlinlang.org/) en l'occurrence. Parce que l'XML, c'est quand même très verbeux et ce n'est pas le plus facile à étendre.

Cela dit :

- Maven est encore très utilisé.
- Il y a souvent plein de façons de faire quelque chose avec Gradle, ce qui peut être déroutant, alors qu'avec Maven il n'y en a souvent qu'une !

---

# 4.3 Gradle - Exemple

<style scoped>
pre {
  font-size: 0.7em;
}
</style>

Un exemple de fichier de build (`build.gradle.kts`) :

```gradle
plugins {
    java
}
group = "re.belv.pokemon"
version = "1.0-SNAPSHOT"
repositories {
    mavenCentral()
}
dependencies {
    testImplementation(platform("org.junit:junit-bom:5.10.0"))
    testImplementation("org.junit.jupiter:junit-jupiter")
}
tasks.test {
    useJUnitPlatform()
}
```

Pour compiler : `gradle build`.

La doc est [là](https://gradle.org/guides/#getting-started).

---

## Fin de la deuxième partie

Des questions ?

---

## Teaser

Dans les parties suivantes, je pense aborder :

- Les collections (des classes telles que `List`, `Set`, `Map` qui permettent de faire plus de trucs que les simples tableaux)
- Les `Stream`s qui rajoutent aux collections la possibilité de faire des `filter()`, `map()` ou `reduce()` que tu as sans doute vu en Javascript
- Les tests avec JUnit
- Les exceptions
- Spring ?