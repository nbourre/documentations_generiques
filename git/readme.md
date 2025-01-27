# Introduction à Git et GitHub <!-- omit in toc -->

# Table des matières <!-- omit in toc -->
- [C'est quoi Git?](#cest-quoi-git)
- [Utiliser la ligne de commande sous Windows](#utiliser-la-ligne-de-commande-sous-windows)
- [Cloner un dépôt GitHub](#cloner-un-dépôt-github)
- [Concepts Importants de Git](#concepts-importants-de-git)
  - [Commit](#commit)
  - [Branche](#branche)
  - [Pousser du code (push)](#pousser-du-code-push)
  - [Ajouter un dépôt distant](#ajouter-un-dépôt-distant)
- [Exercices](#exercices)
- [Connexion de Git à GitHub](#connexion-de-git-à-github)
- [Créer un dépôt sur GitHub et l'utiliser localement](#créer-un-dépôt-sur-github-et-lutiliser-localement)
  - [1. Création d'un dépôt sur GitHub](#1-création-dun-dépôt-sur-github)
- [Dépôt local vs distant](#dépôt-local-vs-distant)
- [Git et la logique d'arbre](#git-et-la-logique-darbre)
- [Authentification avec GitHub](#authentification-avec-github)

---

# C'est quoi Git?

Git est un logiciel de gestion de versions qui permet de suivre les modifications d'un projet, de collaborer avec d'autres personnes et de revenir à des versions précédentes si nécessaire. Il est très utilisé dans le développement logiciel pour gérer le code source et fonctionne en ligne de commande.

GitHub est une plateforme en ligne qui permet d'héberger des dépôts Git afin de faciliter le partage et la collaboration sur des projets.

---

# Utiliser la ligne de commande sous Windows

Windows offre plusieurs moyens d'utiliser Git en ligne de commande. L'une des façons les plus simples est d'utiliser **cmd** (l'invite de commandes). Vous pouvez l'ouvrir de deux manières :

1. **Utiliser le raccourci Windows + R**, taper `cmd`, puis valider.
2. **Dans l'explorateur de fichiers**, naviguez jusqu'au dossier souhaité, puis cliquez dans la barre d'adresse et tapez `cmd`. Cela ouvrira une invite de commandes directement dans le dossier en cours.
   - C'est la méthode que j'utilise personnellement.

---

# Cloner un dépôt GitHub

Cloner un dépôt signifie télécharger une copie d'un projet GitHub sur votre ordinateur. Par défaut, Git crée un dossier contenant le projet dans le répertoire courant.

Exemple : Pour récupérer les projets de cours disponibles sur le dépôt GitHub [https://github.com/nbourre/0sx\_projets\_cours](https://github.com/nbourre/0sx_projets_cours) :

```sh
cd chemin/où/placer/le/projet
```

Ou ouvrez directement `cmd` dans le dossier souhaité, puis exécutez la commande :

```sh
git clone https://github.com/nbourre/0sx_projets_cours.git
```

Une fois terminé, un dossier `0sx_projets_cours` apparaîtra dans le répertoire courant.

---

# Concepts Importants de Git

## Commit
Un **commit** représente un instantané du projet à un moment donné. Chaque commit contient un message décrivant les modifications apportées.

```sh
git commit -m "Description des modifications"
```

Les commits permettent de suivre l'évolution du projet et de revenir à une version antérieure en cas de besoin.

## Branche
Une **branche** est une ligne de développement indépendante. Par défaut, Git utilise une branche appelée `main`, mais vous pouvez en créer d'autres pour travailler sur différentes fonctionnalités sans affecter le code principal.

```sh
git branch ma-nouvelle-branche
```

Pour basculer vers une autre branche :

```sh
git checkout ma-nouvelle-branche
```

## Pousser du code (push)
La commande `git push` permet d'envoyer les modifications locales vers un dépôt distant (GitHub).

```sh
git push origin main
```

Cela mettra à jour le dépôt en ligne avec les changements locaux.

## Ajouter un dépôt distant
Si vous créez un dépôt sur GitHub, GitHub vous donnera directement les commandes pour le cloner ou l'ajouter à un dépôt local existant. Il est souvent plus simple de **créer le dépôt d'abord sur GitHub**, puis de le cloner directement avec :

```sh
git clone https://github.com/utilisateur/mon-projet.git
```

Si vous avez déjà un dépôt local que vous souhaitez lier à GitHub, vous pouvez ajouter un dépôt distant avec :

```sh
git remote add origin https://github.com/utilisateur/mon-projet.git
```

Cela permet ensuite d'envoyer des fichiers avec `git push`.

---

# Exercices

Récupérez les projets du cours disponibles sur GitHub :

1. Ouvrez une ligne de commande dans le dossier où vous souhaitez stocker les projets du cours.
2. Clonez le dépôt avec la commande :

```sh
git clone https://github.com/nbourre/0sx_projets_cours.git
```

3. Vérifiez que le dossier `0sx_projets_cours` a bien été créé et explorez son contenu.

---

# Connexion de Git à GitHub

Si vous souhaitez envoyer du code sur GitHub, vous devez connecter votre Git local à votre compte GitHub.

1. Configurez votre identité Git :

```sh
git config --global user.name "VotreNom"
git config --global user.email "VotreEmailGitHub"
```

2. Ajoutez vos clés SSH ou utilisez une connexion HTTPS avec authentification personnelle.
   - Consultez [la documentation officielle de GitHub](https://docs.github.com/en/authentication) pour configurer votre connexion.

---

# Créer un dépôt sur GitHub et l'utiliser localement

## 1. Création d'un dépôt sur GitHub

1. Rendez-vous sur [GitHub](https://github.com) et connectez-vous.
2. Cliquez sur **New repository**.
3. Donnez-lui un nom et créez-le.
4. GitHub vous donnera plusieurs options :
   - Pour cloner directement le dépôt vide :
   
     ```sh
     git clone https://github.com/utilisateur/mon-projet.git
     ```
   
   - Pour ajouter un dépôt local existant :
     
     ```sh
     git remote add origin https://github.com/utilisateur/mon-projet.git
     git branch -M main
     git push -u origin main
     ```

Votre projet est maintenant accessible sur GitHub!

---

# Dépôt local vs distant

Un **dépôt local** est stocké sur votre ordinateur. Vous pouvez y faire des modifications et sauvegarder l'historique de votre projet.

Un **dépôt distant** est stocké en ligne (sur GitHub, par exemple). Il permet de partager votre code et de collaborer avec d'autres personnes.

Vous pouvez synchroniser les modifications entre ces dépôts en utilisant `git push` (pour envoyer les changements) et `git pull` (pour récupérer les mises à jour depuis GitHub).

---

# Git et la logique d'arbre

Git fonctionne comme un **arbre** avec des branches. Chaque commit représente un **nœud** dans cet arbre, et les branches permettent de travailler sur différentes versions du projet sans affecter la branche principale (`main`).

![alt text](assets/exemple_flux-Base.drawio.svg)

- **Le tronc (main)** : Représente la version stable du projet.
- **Les branches** : Permettent de tester de nouvelles fonctionnalités ou corrections sans affecter `main`.
- **Les fusions (merge)** : Permettent de réintégrer une branche dans `main` une fois le développement terminé.

Cette structure permet une grande flexibilité et facilite le travail collaboratif.

---

# Authentification avec GitHub
1. Générer une clé SSH-ed25519
   1. `ssh-keygen -t ed25519 -C "votre_email@example.com"`
2. Ajouter la clé SSH à l'agent SSH
   1. `eval "$(ssh-agent -s)"`
   2. `ssh-add ~/.ssh/id_ed25519`

[text](assets/VirtualBoxVM_gT4jflTExG.apng)

3. Afficher le contenu de la clé publique SSH
   1. `cat ~/.ssh/id_ed25519.pub`
4. Ajouter la clé publique SSH à GitHub
   1. Atteindre : https://github.com/settings/keys
   2. Cliquer sur "New SSH key"
   3. Donner un nom à la clé
   4. Coller le contenu de la clé publique
5. Pousser le contenu du dépôt local vers GitHub

