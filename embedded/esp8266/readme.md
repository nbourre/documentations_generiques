# Makerfabs ESP8266 WiFi Shield
Ce document est une amélioration et un complément du document [ESP8266 WiFi Shield](https://ca.robotshop.com/products/esp8266-wifi-shield) (RB-Mkf-14) de RobotShop.

![Alt text](assets/esp8266-wifi-shield.webp)

## Table des matières
- [Makerfabs ESP8266 WiFi Shield](#makerfabs-esp8266-wifi-shield)
  - [Table des matières](#table-des-matières)
- [Firmware compatible pour le cours](#firmware-compatible-pour-le-cours)
- [Configuration avec Arduino IDE](#configuration-avec-arduino-ide)
- [Arduino Mega](#arduino-mega)
  - [Branchement au RX1/TX1](#branchement-au-rx1tx1)
  - [Code pour tester la compatibilité](#code-pour-tester-la-compatibilité)
  - [Code d'exemple](#code-dexemple)
- [Mise à jour du firmware](#mise-à-jour-du-firmware)
- [Connexion au module ESP8266](#connexion-au-module-esp8266)
- [Mise à jour du firmware avec ESP8266 Download Tool v3.8.5](#mise-à-jour-du-firmware-avec-esp8266-download-tool-v385)
- [Mise à jour du firmware avec esptool.py](#mise-à-jour-du-firmware-avec-esptoolpy)
  - [Installation de l'outil esptool.py](#installation-de-loutil-esptoolpy)
  - [Sauvegarde du firmware](#sauvegarde-du-firmware)
  - [Effacement du firmware](#effacement-du-firmware)
  - [Téléversement du firmware](#téléversement-du-firmware)
- [Tester le module avec Arduino](#tester-le-module-avec-arduino)
- [Extra](#extra)
- [Références](#références)

# Firmware compatible pour le cours
- [Fichier avec des firmwares compatibles](assets/from_instructables.zip)

---

# Configuration avec Arduino IDE
1. Assurez-vous qu'il n'y a pas de code en cours d'exécution sur l'Arduino.
   - Vous pouvez téléverser un code vide ou l'exemple `Blink`.
2. Empilez le shield sur le dessus de l'Arduino.
3. Vérifiez que le cavalier sur le shield correspond à la broche RX/TX de l'Arduino (TX-TX et RX-RX).
   - Il faut que le cavalier ESP_TX soit relié à la broche TX de l'Arduino et ESP_RX soit relié à la broche RX de l'Arduino. **Ce n'est pas une erreur**, il le faut pour que la communication via le USB se fasse directement au shield. L'Arduino ne sert que de support pour l'électricité à ce point.
   - Généralement, il s'agit des broches 0 et 1 de l'Arduino.
4. Ouvrez l'IDE Arduino.
5. Obtenez la bibliothèque `WiFiEsp` en allant dans le gestionnaire de bibliothèques.
   - Chercher `WiFiEsp` (il ne devrait y avoir qu'un seul choix)
6. Connectez l'Arduino.
7. Sélectionnez la carte Arduino et le bon port COM.
8. Ouvrez le Moniteur Série, sélectionnez `Both NL & CR` et réglez le débit en bauds à 115200.<
9. Tapez la commande `AT` dans la moniteur série. Vous devriez recevoir `OK` en retour.
   - Si vous ne recevez rien, il se peut que n'ayez pas changé les cavaliers sur le shield ou que vous n'avez pas mis `Both NL & CR` dans le moniteur série ou encore le module a été configuré à une autre vitesse de communication.
10. Changez le débit en bauds du module `ESP8266` à 9600 en tapant cette commande :<br/>
    `AT+CIOBAUD=9600` (cela ne doit être fait qu'une fois).
    - Cette action change la vitesse de communication du module `ESP8266` en utilisant une commande `AT`. Il faudra en prendre considération lors de configuration future du module.
11. Déconnectez tout.
12. Testez la configuration en envoyant la commande `AT` via le moniteur série.
    - Assurez-vous d'avoir configuré le moniteur série à la nouvelle vitesse.
13. Déconnectez tout.

> ***Important*** : Cette partie semble fonctionner que pour l'Arduino Uno. Pour l'Arduino Mega, il faut désactiver les cavaliers et brancher sur les RX1/TX1. En effet, `SoftwareSerial` ne fonctionne pas pour les broches qui sont sur le shield avec le Mega ([Source](https://docs.arduino.cc/learn/built-in-libraries/software-serial)).

14. Changez le cavalier de sorte que le RX du shield soit relié à la broche n°3 de l'Arduino.
    - ESP_RX doit être relié à la broche 3 de l'Arduino.
    - Nous allons utiliser `SoftwareSerial` pour communiquer avec le module `ESP8266`
15. Changez le cavalier de sorte que le TX du shield soit relié à la broche n°2 de l'Arduino.
    - ESP_TX doit être relié à la broche 2 de l'Arduino. 
    - Nous allons utiliser `SoftwareSerial` pour communiquer avec le module `ESP8266`

> **Note :** Pour les étapes 11 et 12, cela dépendra du code envoyer dans l'Arduino.

---

# Arduino Mega

## Branchement au RX1/TX1

- Désactiver les cavaliers en les plaçant en parallèle (voir image ci-dessous).

![Alt text](assets/makerfabs_wifi_mega.jpg)
- Brancher le RX du shield sur la broche TX1 de l'Arduino Mega.
- Brancher le TX du shield sur la broche RX1 de l'Arduino Mega.

---

## Code pour tester la compatibilité
Voici l'exemple de code `CheckFirmware` de la librairie `WifiEspAT`. Il permet de valider la compatibilité du module `ESP8266` avec la librairie `WiFiEspAT`. Il faudra ouvrir le moniteur série de Arduino IDE à 115200 bauds pour voir le résultat.

```cpp

#include <WiFiEspAT.h>


#define AT_BAUD_RATE 115200

void setup() {

  Serial.begin(115200);
  while (!Serial);

  Serial1.begin(AT_BAUD_RATE);
  WiFi.init(Serial1);

  if (WiFi.status() == WL_NO_MODULE) {
    Serial.println("Communication with WiFi module failed!");
    // don't continue
    while (true);
  }

  char ver[10];
  int major = 0;
  int minor = 0;
  if (WiFi.firmwareVersion(ver)) {
    Serial.print("AT firmware version ");
    Serial.println(ver);
    char* tok = strtok(ver, ".");
    major = atoi(tok);
    tok = strtok(NULL, ".");
    minor = atoi(tok);
    if (major == 2 && minor == 0) {
      Serial.println("AT firmware version 2.0 doesn't support passive receive mode and can't be used with the WiFiEspAt library");
    } else if (major < 1 || (major == 1 && minor < 7)) {
      Serial.println("WiWiEspAT library requires at least version 1.7.0 of AT firmware (but not 2.0)");
    } else {
      Serial.println("AT firmware is OK for the WiFiEspAT library.");
#ifdef WIFIESPAT1
      if (major > 1) {
        Serial.println("For AT firmware version 2 comment out #define WIFIESPAT1 in EspAtDrvTypes.h");
      }
#else
      if (major == 1) {
        Serial.println("For AT firmware version 1 add #define WIFIESPAT1 in EspAtDrvTypes.h");
      }
#endif
    }
  } else {
    Serial.println("Error getting AT firmware version");
  }


}

void loop() {
}
```

---

## Code d'exemple

Voici un code d'exemple qui devrait permettre au Arduino Mega de se connecter à un réseau WiFi. 

```cpp
#include <WiFiEspAT.h>

// Mettre à 1 si le fichier arduino_secrets.h est présent
#define HAS_SECRETS 0  

#if HAS_SECRETS
#include "arduino_secrets.h"
/////// SVP par soucis de sécurité, mettez vos informations dans le fichier arduino_secrets.h

// Nom et mot de passe du réseau wifi
const char ssid[] = SECRET_SSID;
const char pass[] = SECRET_PASS;

#else
const char ssid[] = "ton_ssid";  // your network SSID (name)
const char pass[] = "ton_mot_de_passe";  // your network password (use for WPA, or use as key for WEP)

#endif

#define AT_BAUD_RATE 115200

int blinkRate = 500;

void setup() {
  pinMode(LED_BUILTIN, OUTPUT);

  Serial.begin(115200);
  while (!Serial);

  Serial1.begin(AT_BAUD_RATE);
  WiFi.init(&Serial1);

  // Cela peut prendre un certain temps pour que le module wifi soit prêt
  // Voir 1 minute dans la documentation
  if (WiFi.status() == WL_NO_MODULE) {
    Serial.println();
    Serial.println("La communication avec le module WiFi a échoué!");
    // ne pas continuer
    errorState(2, 1);
  }

  WiFi.disconnect();  // pour effacer le chemin. non persistant

  WiFi.setPersistent();  // définir la connexion WiFi suivante comme persistante

  WiFi.endAP();  // pour désactiver le démarrage automatique persistant AP par défaut au démarrage

  // décommentez ces lignes pour une adresse IP statique persistante. définissez des adresses valides pour votre réseau
  // Pour l'adresse du cégep, la plage est 172.22.0.0 où les derniers octets sont entre 1 et 254
  // IPAddress ip(192, 168, 1, 9);
  // IPAddress gw(192, 168, 1, 1);
  // IPAddress nm(255, 255, 255, 0);
  // WiFi.config(ip, gw, gw, nm);

  Serial.println();
  Serial.print("Tentative de connexion à SSID: ");
  Serial.println(ssid);

  int status = WiFi.begin(ssid, pass);

  if (status == WL_CONNECTED) {
    Serial.println();
    Serial.println("Connecté au réseau WiFi.");
    printWifiStatus();
  } else {
    WiFi.disconnect();  // supprimer la connexion WiFi
    Serial.println();
    Serial.println("La connexion au réseau WiFi a échoué.");
    blinkRate = 100;
  }
}

void loop() {
  static unsigned long lastBlink = 0;
  if (millis() - lastBlink >= blinkRate) {
    lastBlink = millis();
    digitalWrite(LED_BUILTIN, !digitalRead(LED_BUILTIN));
  }
}

void printWifiStatus() {

  // imprime le SSID du réseau auquel vous êtes connecté:
  char ssid[33];
  WiFi.SSID(ssid);
  Serial.print("SSID: ");
  Serial.println(ssid);

  // imprime le BSSID du réseau auquel vous êtes connecté:
  uint8_t bssid[6];
  WiFi.BSSID(bssid);
  Serial.print("BSSID: ");
  printMacAddress(bssid);

  uint8_t mac[6];
  WiFi.macAddress(mac);
  Serial.print("MAC: ");
  printMacAddress(mac);

  // imprime l'adresse IP de votre carte:
  IPAddress ip = WiFi.localIP();
  Serial.print("Adresse IP: ");
  Serial.println(ip);

  // imprime la force du signal reçu:
  long rssi = WiFi.RSSI();
  Serial.print("force du signal (RSSI):");
  Serial.print(rssi);
  Serial.println(" dBm");
}

// Imprime l'adresse MAC
void printMacAddress(byte mac[]) {
  for (int i = 5; i >= 0; i--) {
    if (mac[i] < 16) {
      Serial.print("0");
    }
    Serial.print(mac[i], HEX);
    if (i > 0) {
      Serial.print(":");
    }
  }
  Serial.println();
}

// Fonction affichant un état d'erreur avec 2 paramètres pour code d'erreur
// Cela permet de clignoter la DEL avec un code pour faciliter le débogage pour
// l'utilisateur. On ne sort jamais de cette fonction.
// Le programmeur doit définir la signification des codes d'erreur.
//
// Exemple de code d'erreur:
//   errorState (2, 3) <-- (clignote 2 fois, pause, clignote 3 fois)
void errorState(int codeA, int codeB) {
  const int rate = 100;
  const int pauseBetween = 500;
  const int pauseAfter = 1000;

  // On ne sort jamais de cette fonction
  while (true) {
    for (int i = 0; i < codeA; i++) {
      digitalWrite(LED_BUILTIN, HIGH);
      delay(rate);
      digitalWrite(LED_BUILTIN, LOW);
      delay(rate);
    }
    delay(pauseBetween);
    for (int i = 0; i < codeB; i++) {
      digitalWrite(LED_BUILTIN, HIGH);
      delay(rate);
      digitalWrite(LED_BUILTIN, LOW);
      delay(rate);
    }
    delay(pauseAfter);

    Serial.print("Erreur : ");
    Serial.print(codeA);
    Serial.print(".");
    Serial.println(codeB);
  }
}

```

Dans le moniteur série, vous devriez avoir un message comme celui-ci :

```text
[WiFiEsp] Initializing ESP module
[WiFiEsp] Initilization successful - 1.5.4
Attempting to connect to WPA SSID: YOUR_NETWORK_NAME
[WiFiEsp] Connected to YOUR_NETWORK_NAME
You're connected to the network
SSID: YOUR_NETWORK_NAME
IP Address: 10.79.83.219
Signal strength (RSSI):-52 dBm

Starting connection to server...
[WiFiEsp] Connecting to arduino.cc
Connected to server

Disconnecting from server...
```

---

# Mise à jour du firmware

Attention! Cette partie est très dangereuse. Si vous ne savez pas ce que vous faites, vous risquez de brûler votre module `ESP8266`. Il est donc fortement recommandé de sauvegarder le firmware actuel avant de le modifier.

# Connexion au module ESP8266
Si vous avez un module qui n'a pas de port USB, vous devez utiliser un adaptateur USB-UART. Vous pouvez en trouver sur [Amazon](https://a.co/d/1gRKGrE).

![Alt text](assets/usb_uart.jpg)

Pour le branchement, il faut relier les broches suivantes :

| USB-UART | ESP8266 |
|----------|---------|
| GND      | GND     |
| RX       | TX      |
| TX       | RX      |
| 3.3V     | 3.3V*    |

> **\*** Si vous utilisez un module avec un régulateur de tension par exemple un shield Arduino, vous pouvez utiliser le 5V de l'adaptateur et le brancher dans l'entrée du voltage.

Il faudra aussi brancher le port **GPIO0 sur GND** pour que le module entre en mode flash. Pour cela, vous pouvez utiliser un jumper ou un bouton relié à la masse.

![Alt text](assets/ESP8266-ESP-12E-chip-pinout-gpio-pin.webp)

![Alt text](assets/branchement_gpio0.jpg)

# Mise à jour du firmware avec ESP8266 Download Tool v3.8.5
[Source de l'article original](https://www.makerfabs.cc/article/esp8266-wifi-shield-firmware-upgrade-v1-0.html)

Vous pouvez aussi utiliser l'outil **ESP8266 Download Tool**. Dans le cas présent, nous avons utilisé la version 3.8.5  pour téléverser le firmware.

> **Note :** Avant d'effectuer la mise à jour, il est fortement recommandé de sauvegarder le firmware actuel. Pour cela, vous pouvez vous référer à la partie [Mise à jour du firmware avec esptool.py](#mise-à-jour-du-firmware-avec-esptoolpy)

Il suffit de lancer l'application et de suivre les étapes suivantes :

![Alt text](assets/flash_download_tool_v3.8.5_A.png)

1. Cliquer sur `Developer Mode`
2. Cliquer sur `ESP8266 DownloadTool`
   ![Alt text](assets/flash_download_tool_v3.8.5_B.png)
3. Sélectionner le port COM de votre module
4. S'assurer que GPIO0 est sur GND
5. Toucher rapidement la broche `RST` avec le ground pour mettre le module en mode flash

![Alt text](assets/rst_pin.gif)
   On ne voit pas dans la vidéo, mais la DEL bleu du module s'allume très brièvement.
6. S'assurer que rien n'est coché dans la liste des fichiers

![Alt text](assets/flash_download_tool_v3.8.5_C.png)
   
7. Cliquer sur `Start`
   Après avoir sur `Start`, l'application cherchera et affichera les informations du module
   
![Alt text](assets/flash_download_tool_v3.8.5_D.png)
   
8. Récupérer le firmware téléchargé dans la source de l'article original
9. Ajouter le firmware à fin de la liste des fichiers
10. Mettre l'adresse de début à `0x00000`
11. Cocher la case devant le fichier
12. Sélectionner les mêmes options que dans les captures d'écran
13. Appuyer sur `Start`
    1.  Le téléversement du firmware peut prendre plusieurs minutes. Une fois terminé, vous devriez avoir un message comme celui-ci :
        `is stub and send flash finish`

Pour tester le module, vous n'avez qu'à suivre les étapes de la section [Configuration avec Arduino IDE](#configuration-avec-arduino-ide).

# Mise à jour du firmware avec esptool.py

## Installation de l'outil esptool.py
Avant de modifier le firmware, il faut le sauvegarder. Pour ce faire, nous avons beesoin de l'outil `esptool.py` qui est disponible [ici](https://github.com/espressif/esptool). Il suffit de télécharger ou cloner le dépôt et de lancer la commande suivante :

Dans le dossier `esptool`, lancez la commande suivante pour effectuer l'installation :

```bat
python setup.py install
```

> **Note :** Dépendant de votre installation de python, vous devrez peut-être lancer la commande dans un terminal en mode administrateur.

## Sauvegarde du firmware
La première étape sera de lire la dimension du firmware actuel. Pour cela, il faut lancer la commande suivante :

```bat
python esptool.py --port COM3 flash_id
```

- `COM3` est le port de votre module. Il peut être différent de celui-ci.

Voici un exemple de résultat :

```text
esptool.py v4.6-dev
Serial port COM13
Connecting...
Detecting chip type... Unsupported detection protocol, switching and trying again...
Connecting...
Detecting chip type... ESP8266
Chip is ESP8266EX
Features: WiFi
Crystal is 26MHz
MAC: 4c:75:25:26:7a:04
Stub is already running. No upload is necessary.
Manufacturer: 5e
Device: 4016
Detected flash size: 4MB
Hard resetting via RTS pin...

```

On peut voir que le firmware fait 4MB. Il faut maintenant le sauvegarder. Pour cela, il faut lancer la commande suivante :

```bat
python esptool.py --port COM3 read_flash 0x00000 0x400000 esp8266_backup.bin
```

Cette action peut prendre plusieurs minutes. Une fois terminée, vous devriez avoir un fichier `esp8266_backup.bin` dans le dossier `esptool`.

Voici un exemple de résultat :

```text	
esptool.py v4.6-dev
Serial port COM13
Connecting...
Detecting chip type... Unsupported detection protocol, switching and trying again...
Connecting...
Detecting chip type... ESP8266
Chip is ESP8266EX
Features: WiFi
Crystal is 26MHz
MAC: 4c:75:25:26:7a:04
Stub is already running. No upload is necessary.
4194304 (100 %)
4194304 (100 %)
Read 4194304 bytes at 0x00000000 in 392.8 seconds (85.4 kbit/s)...
Hard resetting via RTS pin...
```

## Effacement du firmware
Avant d'effectuer la mise à jour du firmware, il faut effacer le contenu de la mémoire flash. Pour cela, il faut lancer la commande suivante :

```bat
python esptool.py --port COM3 erase_flash
```

Cela prendra quelques secondes et on ne verra pas de lumière sur le module. Une fois terminé, vous devriez avoir un message comme celui-ci :

```text
esptool.py v4.6-dev
Serial port COM13
Connecting...
Detecting chip type... Unsupported detection protocol, switching and trying again...
Connecting...
Detecting chip type... ESP8266
Chip is ESP8266EX
Features: WiFi
Crystal is 26MHz
MAC: 4c:75:25:24:ca:e8
Stub is already running. No upload is necessary.
Erasing flash (this may take a while)...
Chip erase completed successfully in 12.9s
Hard resetting via RTS pin...
```

## Téléversement du firmware
Après avoir faites toutes les étapes précédentes, on peut passer au téléversement du firmware. Pour cela, il faut lancer la commande suivante :

Pour un module de 4MB :
```bat
python esptool.py --port COM3 write_flash -fs 4MB -fm dout 0x0 nouveauFirmware.bin
```

Pour un module de 2MB :
```bat
python esptool.py --port COM3 write_flash -fs 2MB -fm dout 0x0 nouveauFirmware.bin
```

- `COM3` est le port de votre module. Il peut être différent de celui-ci.
- La taille du firmware dépend de la taille de votre module. Si vous avez un module de 1MB, il faut utiliser la commande pour un module de 1MB.
- *nouveauFirmware.bin* est le chemin vers le fichier du firmware.

---

# Tester le module avec Arduino

Étant donné que nous utilisons la librarie `WifiEspAt`, nous pouvons tester la compatilibilité du module avec l'exemple de projet `CheckFirmware` de la librairie. Ce projet se retrouve dans `Examples/WifiEspAt/Tools/CheckFirmware`. Sur le Mega, il faut changer le port de communication pour le port RX1/TX1.

Si le module est compatible, vous devriez avoir un message comme celui-ci :

```text
AT firmware version 1.7.1.0
AT firmware is OK for the WiFiEspAT library.
```

---

# Extra

- Gabarit 3D à imprimer pour protéger les broches et n'exposer que GPIO0 et RST. 
[Fichier STL](assets/esp-12f%20flashing%20frame.stl)

![Alt text](assets/esp8266_flashing_frame.jpg)

---

# Références
- [Site officiel - OAS8266WF](https://www.makerfabs.com/esp8266-wifi-shield.html)
