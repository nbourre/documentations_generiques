# Workflows pour Moodle <!-- omit in toc -->

- [Évaluation, Quiz et Test](#évaluation-quiz-et-test)
  - [Créer un test en format GIFT avec médias](#créer-un-test-en-format-gift-avec-médias)
    - [Insérer des images dans un test GIFT](#insérer-des-images-dans-un-test-gift)
    - [Pour catégoriser les questions](#pour-catégoriser-les-questions)
  - [Importer les questions GIFT dans Moodle](#importer-les-questions-gift-dans-moodle)
  - [Créer un test sur Moodle](#créer-un-test-sur-moodle)
  - [Ajouter des questions au test](#ajouter-des-questions-au-test)
  - [Correction des évaluations](#correction-des-évaluations)
    - [Correction manuelle](#correction-manuelle)
- [Gestion des utilisateurs](#gestion-des-utilisateurs)
  - [Sauvegarder les données des étudiants](#sauvegarder-les-données-des-étudiants)
  - [Désinscrire les étudiants en lot](#désinscrire-les-étudiants-en-lot)


# Évaluation, Quiz et Test

## Créer un test en format GIFT avec médias
1. Écrire les questions dans un fichier « .gift »
2. Modifier l’extension pour « .txt »
   - [Source](https://docs.moodle.org/401/en/Gift_with_medias_format) dans la section « Tips »
3. Compresser le dossier avec le fichier « .gift » ainsi que le dossier des images

### Insérer des images dans un test GIFT
1. Créer un dossier « img »
2. Mettre les images à l’intérieur.
3. Utiliser la balise `<img src\="@@PLUGINFILE@@/img/fichier" /><br/>`

### Pour catégoriser les questions
- Ajouter la ligne « $CATEGORIE: NomExamen/catégorie/sous-catégorie… » entre chaque catégorie de questions.

## Importer les questions GIFT dans Moodle
1. Atteindre le cours
2. Activer le mode d’édition
3. Aller dans « Plus --> Banque de questions »
4. Cliquer sur « Questions » et sélectionner « Importer »
5. Sélectionner « GIFT »
   1. Ou « Format Gift avec des fichiers média » si applicable
6. Choisir le fichier `.zip` à importer
   1. Fichier `.gift` s’il n’y a pas d’images

## Créer un test sur Moodle
- Créer le test
- Masquer le contenu jusqu’au moment d’ouvrir l’évaluation
- Marquer la date de l’ouverture
  - [Optionnel] : Marquer le temps disponible
- Marquer le nombre de tentatives que les étudiants peuvent faire
  - 1 généralement

## Ajouter des questions au test
1. Dans le test, atteindre la section « Questions ».
2. Cliquer sur « Ajouter »
3. Sélectionner les options désirées
4. Ajuster les points au besoin

## Correction des évaluations

### Correction manuelle
1. Aller dans le test
2. Cliquer sur l'onglet "Résultats"
3. Sélectionner "Évaluation manuelle"
4. Sélectionner "Tout évaluer" pour chaque question


# Gestion des utilisateurs
## Sauvegarder les données des étudiants
1. Aller dans le cours
2. Aller dans la section « Participants »
3. Sélectionner les étudiants à sauvegarder
4. Dans la liste déroulante dans le bas de la page, sélectionner « Télécharger les données au format » qui vous convient.

## Désinscrire les étudiants en lot
Après un certain temps, il est possible que certains utilisateurs ne soient plus actifs dans le cours. Il est possible de les désinscrire en lot.

> **Important**
> 
> Avant de désinscrire les utilisateurs, il est préférable de faire une sauvegarde des utilisateurs inscrits dans le cours. Voir cette section.

Dans l'installation actuel au Cégep, il semble y avoir un bogue avec l'option de désinscription en lot. Il est possible de désinscrire les utilisateurs en lot en utilisant la méthode suivante:

1. Aller dans le cours
2. Aller dans la section « Participants »
3. Dans la liste déroulante qui est inscrit "Utilisateurs inscrits", sélectionner "Méthode d'inscription"
   
![Alt text](img/moodle_methode_insc.gif)
<br />

4. Supprimer la méthode d'inscription.
   - **Attention!** Cette action va désinscrire tous les utilisateurs du cours qui ont été inscrits par cette méthode.



