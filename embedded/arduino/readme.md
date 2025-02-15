# Arduino

# Programmer via le ICSP
Le ground est la broche la plus éloignée du port USB.

La commande suivante permet de reprogrammer le microcontrolleur Atmega2560 :

```bash
# La commande normale
avrdude -c usbasp -p m2560 -e -U flash:w:Fade.ino.with_bootloader.hex

# La commande avec un délai de 100ms entre les instructions pour ralentir la programmation qui est parfois trop rapide
avrdude -c usbasp -p m2560 -B 100 -e -U flash:w:Fade.ino.with_bootloader.hex
```

# Ressuciter le USB
Si le port USB ne fonctionne plus et que le microcontrolleur est encore fonctionnel, il est possible de le reprogrammer via le ICSP s'il y a un Atmega16U2 ou Atmega8U2 pour la conversion USB/Serial.

Le ground est la broche la plus proche du port USB.

La commande suivante permet de reprogrammer le microcontrolleur Atmega16U2 d'une carte Arduino Mega 2560:

```bash
avrdude -c usbasp -p m16u2 -B 10 -U flash:w:Arduino-usbserial-atmega16u2-Mega2560-Rev3.hex:i
```