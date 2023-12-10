# Commandes en lien avec la réseautique

# Visualiser la consommation de bande passante par processus avec `nethogs`

```bash
sudo apt install nethogs
sudo nethogs
```

# Visualiser les connexions réseau avec `netstat`

```bash
sudo netstat -tulpn
```

# Installer openvpn

Utiliser le script à l'adresse suivante : https://git.io/vpn

**Valider le contenu du script avant l'exécution!!**

```bash
wget https://git.io/vpn -O openvpn-install.sh
chmod +x openvpn-install.sh
sudo ./openvpn-install.sh
```

- Cela va créer un fichier `.ovpn` dans le dossier `/root` qui contient les informations de connexion.
- Copier ou importer le fichier `.ovpn` dans le dossier du client.
- S'assurer que le port 1194 est ouvert sur le pare-feu du serveur.