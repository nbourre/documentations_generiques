# Documentation pour la programmation en générale <!-- omit in toc -->
Ces documents sont des trucs et astuces pour la programmation en générale. Il n'y a pas vraiment de langage de programmation en particulier sauf si spécifié.

# Table des matières <!-- omit in toc -->
- [Liste de vérifications générales pour la programmation](#liste-de-vérifications-générales-pour-la-programmation)
- [C++](#c)
- [Arduino](#arduino)
  - [Gestion des topics MQTT](#gestion-des-topics-mqtt)

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

# C++
- La portée des variables est adéquates, i.e. éviter des variables globales si ce n’est pas nécessaires.
  - Par exemple, utilise une variable statique si celle-ci n’est pas nécessaire à l’extérieur de la fonction, mais qui doit être gardé en mémoire à chaque appelle
  - `lastTime` ou `previousMillis` sont de bon exemple où l'on pourrait utiliser une variable statique à l'intérieur d'une fonction.
- Tu as mis `#pragma once` à la première ligne de chacun de tes « .h ». On appelle cela un « header guard ». Cela permet d’éviter les problèmes de double inclusion.
  - L’alternative c’est de faire ce code
    ```cpp
    #ifndef _NOMCLASSE_H_
    #define _NOMCLASSE_H_
    // Contenu du fichier .h
    #endif
    ```


# Arduino
- Ta boucle est simple à comprendre et appelle des fonctions ou méthodes.
- Tu n’as pas utilisé la fonction `delay()` (ou equivalent) dans le code de la boucle.
  - Je n'accepte les appels de `delay()` que dans la configuration.
- Tu utilises des tâches ou état pour gérer ton code.
- Ton code est indenté pour faciliter la lecture de celui-ci
  - Dans l’éditeur Arduino, tu peux aller dans le menu « Édition » et faire « Autoformater ».
  Touches raccourcis : <kbd>CTRL</kbd> + <kbd>T</kbd>

## Gestion des topics MQTT
Pour limiter la fragmentation de la mémoire, il est recommandé de ne pas utiliser le type `String` pour les topics MQTT. Utilise plutôt des tableaux de caractères (`char[]`).

Voici une méthode qui utilise du mapping pour les topics MQTT:

Définition des handlers:

```cpp
#include <SafeString.h>

typedef void (*TopicHandler)(byte* payload, unsigned int length);

struct TopicFunctionMap {
  const char* topic;
  TopicHandler handler;
};

void handleColour(byte* payload, unsigned int length) {
  // Logique pour la couleur
}

void handleMotor(byte* payload, unsigned int length) {
  // Logique pour le moteur
}

// Initialisation des topics:
TopicFunctionMap handlers[] = {
  {"colour", handleColour},
  {"motor", handleMotor}
};
const int handlersSize = sizeof(handlers) / sizeof(handlers[0]);

//Gestion des topics:
void topicManagement(char* topic, byte* payload, unsigned int length) {
  // Créez un SafeString à partir du topic C-string
  createSafeString(safeTopic, 20, topic); 

  for (int i = 0; i < handlersSize; i++) {
    if (safeTopic == handlers[i].topic) {
      handlers[i].handler(payload, length);
      return;
    }
  }

  Serial.println("Topic non géré: ");
  Serial.println(topic);
}

//Intégration dans mqttEvent:
void mqttEvent(char* topic, byte* payload, unsigned int length) {
  char* specificTopic = topic + strlen("prof/"); // Ignorer le préfixe

  topicManagement(specificTopic, payload, length);
}
```