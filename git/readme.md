# Workflow en lien avec GitHub

# Authentification avec GitHub
1. Générer une clé SSH-ed25519
   1. `ssh-keygen -t ed25519 -C "votre_email@example.com"`
2. Ajouter la clé SSH à l'agent SSH
   1. `eval "$(ssh-agent -s)"`
   2. `ssh-add ~/.ssh/id_ed25519`
3. Afficher le contenu de la clé publique SSH
   1. `cat ~/.ssh/id_ed25519.pub`
4. Ajouter la clé publique SSH à GitHub
   1. Atteindre : https://github.com/settings/keys
   2. Cliquer sur "New SSH key"
   3. Donner un nom à la clé
   4. Coller le contenu de la clé publique
5. Pousser le contenu du dépôt local vers GitHub

