# Note pour Retropie

- [Note pour Retropie](#note-pour-retropie)
- [Fichier de configuration EmulationStation](#fichier-de-configuration-emulationstation)
  - [Changer le konami code](#changer-le-konami-code)
    - [Si bogue](#si-bogue)
- [Checkist pour Arcade Ubuntu](#checkist-pour-arcade-ubuntu)


# Fichier de configuration EmulationStation
Le fichier `es_settings.cfg` est situé dans le dossier `/opt/retropie/configs/all/emulationstation/`.

## Changer le konami code
- Modifier la ligne suivante :
  ```xml
  <string name="UIMode_passkey" value="uuddlrlrba" />
  ```

- Redémarrer EmulationStation

### Si bogue
Modifier les permissions du dossier `configs` :
```bash
sudo chown -R pi:pi configs/
```

# Checkist pour Arcade Ubuntu
- [ ] Désactiver les mises à jour automatiques
- [ ] Faire un cron à tous les X temps pour les mises à jour