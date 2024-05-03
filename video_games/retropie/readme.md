# Note pour Retropie

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
