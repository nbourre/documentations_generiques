# Exporter sur Retropie Linux-PC

Pour exporter un jeu Godot sur Retropie, il faut d'abord le compiler pour Linux-PC. Pour cela, il faut installer le module d'exportation pour Linux-PC dans Godot.

# Installer godot-engine sur Retropie
1. En ssh, se connecter à Retropie
2. Dans le dossier `~`, cloner le dépôt Godot : `git clone https://github.com/nbourre/RetroPie-Godot-Engine-Emulator.git`
3. Se déplacer dans le dossier `RetroPie-Godot-Engine-Emulator` : `cd RetroPie-Godot-Engine-Emulator`
4. Changer les permissions du script d'installation : `chmod +x setup-godot-engine-scriptmodule.sh`
5. Exécuter le script d'installation : `./setup-godot-engine-scriptmodule.sh`

Voici le bloc de code pour le script d'installation :

```bash
git clone https://github.com/nbourre/RetroPie-Godot-Engine-Emulator.git
cd RetroPie-Godot-Engine-Emulator
chmod +x setup-godot-engine-scriptmodule.sh
./setup-godot-engine-scriptmodule.sh --install
```

6. Exécuter le script de configuration de RetroPie : `sudo ~/RetroPie-Setup/retropie_setup.sh`
7. Aller dans "Manage packages" -> "Manage experimental packages" -> "godot-engine" -> "Install from source"
8. Attendre la fin de l'installation
9. Revenir au menu principal et quitter le script de configuration
10. Aller dans le dossier `opt/retropie/emulators/godot-engine`
11. Modifier les permissions du fichier `godot_4.3.x86_64` : `chmod +x godot_4.3.x86_64`

# Installer le module d'exportation pour Linux-PC
Editor -> Manage Export Templates -> Download -> Linux/X11

# Exporter l'exécuteur Godot Linux-PC
1. Ouvrir l'outil d'exportation : Project -> Export
2. Cliquer sur "Add..." et sélectionner "Linux/X11"
3. Sélectionner l'architecture de votre système (32 ou 64 bits)
4. Cliquer sur "Export Project" pour l'exécutable
5. Cliquer sur "Export PCK/ZIP" pour les ressources

# Envoyer les fichiers sur Retropie
1. Dans le dossier `roms/godot-engine`, créer un dossier pour votre jeu
2. Copier le fichiers PCK dans le nouveau dossier



