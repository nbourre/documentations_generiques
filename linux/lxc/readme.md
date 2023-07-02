# Documentation en lien avec LXC

# Création d'un conteneur
1. Création du conteneur `lxc launch ubuntu:22.04 jupyterhub`
   - `ubuntu:22.04` est le nom de l'image
   - `jupyterhub` est le nom du conteneur
2. Ajout des règles de pare-feu `ufw`

   ```
   sudo ufw allow in on lxdbr0
   sudo ufw route allow in on lxdbr0
   sudo ufw route allow out on lxdbr0
   ```

3. Connexion au conteneur `lxc exec jupyterhub -- sudo --login --user ubuntu`
   - `exec` permet d'exécuter une commande dans le conteneur indiqué
4. Test de la connexion internet `ping 8.8.8.8`


## Donner les droits à l'utilisateur pour utiliser ajouter des connexions réseau
`echo "$(id -un) veth lxcbr0 10" | sudo tee -a /etc/lxc/lxc-usernet`

## Création d'un conteneur avec un fichier de configuration

- Créer un dossier de configuration

```bash
mkdir -p ~/.config/lxc
cp /etc/lxc/default.conf ~/.config/lxc/default.conf
MS_UID="$(grep "$(id -un)" /etc/subuid  | cut -d : -f 2)"
ME_UID="$(grep "$(id -un)" /etc/subuid  | cut -d : -f 3)"
MS_GID="$(grep "$(id -un)" /etc/subgid  | cut -d : -f 2)"
ME_GID="$(grep "$(id -un)" /etc/subgid  | cut -d : -f 3)"
echo "lxc.idmap = u 0 $MS_UID $ME_UID" >> ~/.config/lxc/default.conf
echo "lxc.idmap = g 0 $MS_GID $ME_GID" >> ~/.config/lxc/default.conf
```

 # À savoir
- Le fichier de config par défaut se retrouve dans `/etc/lxc/default.conf`

# Références
- [LXC : Getting started](https://linuxcontainers.org/lxc/getting-started/)
