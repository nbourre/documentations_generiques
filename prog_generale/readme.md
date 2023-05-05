# Documentation pour la programmation en générale <!-- omit in toc -->
Ces documents sont des trucs et astuces pour la programmation en générale. Il n'y a pas vraiment de langage de programmation en particulier sauf si spécifié.

# Table des matières <!-- omit in toc -->
- [Liste de vérifications générales pour la programmation](#liste-de-vérifications-générales-pour-la-programmation)
  - [C++](#c)
  - [Arduino](#arduino)

# Liste de vérifications générales pour la programmation
Voici une liste de vérifications pour t’aider à réviser ton code
- Tu utilises des variables ou constantes et non des valeurs numériques directement.
  - Par exemples, on utilise des constantes pour les numéros de broches ou des variables pour les limites.
- Tu utilises des noms de variable et fonction significatifs.
  - Exemples invalides : « `lastTime1`, `lastTime2`, etc. »
- Tes variables n'ont pas d'accent ou d'autres caractères spéciaux.
  - Les seuls caractères acceptés sont les lettres, les chiffres et le caractère souligné « `_` ».
- Tu as mis des commentaires pertinents.
- Ton code est indenté pour faciliter la lecture de celui-ci

## C++
- La portée des variables est adéquates, i.e. éviter des variables globales si ce n’est pas nécessaires.
  - Par exemple, utilise une variable statique si celle-ci n’est pas nécessaire à l’extérieur de la fonction, mais qui doit être gardé en mémoire à chaque appelle
  - `lastTime` ou `previousMillis` sont de bon exemple où l'on pourrait utiliser une variable statique à l'intérieur d'une fonction.
- Tu as mis « #pragma once » à la première ligne de chacun de tes « .h ». On appelle cela un « header guard »
  - L’alternative c’est de faire ce code
    ```cpp
    #ifndef _NOMCLASSE_H_
    #define _NOMCLASSE_H_
    // Contenu du fichier .h
    #endif
    ```


## Arduino
- Ta boucle est simple à comprendre et appelles des fonctions ou méthodes.
- Tu n’as pas utilisé la fonction `delay()` dans le code de la boucle.
  - L’appel de `delay()` est acceptable dans la configuration.
- Tu utilises des tâches ou état pour gérer ton code.

- Ton code est indenté pour faciliter la lecture de celui-ci
  - Dans l’éditeur Arduino, tu peux aller dans le menu « Édition » et faire « Autoformater ».
  Touches raccourcis : <kbd>CTRL</kbd> + <kbd>T</kbd>

