# Raspberry Pi

## Wat is een Raspberry Pi?

**Een Raspberry Pi is een minicomputer.** De minicomputer is ongeveer zo net zo groot als een creditcard en past makkelijk in je broekzak. Het is een printplaatje en bevat alle belangrijke componenten die een computer nodig heeft, zoals de ARM-processor en alle benodigde aansluitingen.

Stel dat jij een monitor, muis en een toetsenbord zou aansluiten op de Raspberry Pi, dan heb je gewoon een echte computer. Zoals we net al zeiden, bevat de Raspberry Pi veel aansluitingen die een “gewone” computer ook heeft, zoals USB, UTP poort (Internetpoort), HDMI en micro USB (Oplaadpoort/voeding).

De Raspberry Pi heeft geen opslag en daarom heb je wel een micro-SD kaartje nodig, anders kan je er weinig mee. Het micro-SD kaartje kan je makkelijk in de micro-SD slot/gleuf doen.

Op dit micro-SD kaartje zet je bijvoorbeeld software die je zelf hebt geprogrammeerd. Hiermee stuur je bijvoorbeeld motortjes aan of je verwerkt data van sensoren. Het is bijvoorbeeld ook mogelijk om een besturingssysteem erop te zetten, dat is wat makkelijker werken als je het aansluit op een monitor.

Misschien kan je je nu al een beetje indenken dat er heel veel mogelijkheden zijn met een Raspberry Pi, maar daar komen we later in dit artikel op terug.

Het is ook wel leuk om even het verleden in te duiken. Aan de universiteit van Cambridge is de Raspberry Pi ontwikkeld. **Het was vooral bedoeld om kinderen enthousiast te maken om te gaan programmeren. De Raspberry Pi is heel goedkoop vergeleken met een “echte” computer en daarom werd het nog toegankelijker.**

Door de goedkope prijs was ook het doel om mensen in derdewereldlanden meer kennis te laten maken met een (mini)computer. In 2012 kwam de eerste Raspberry Pi Model B op de markt.

## Models

| Raspberry Pi Platform | CPU                                             | RAM          | I/O Ports                                                            | Price |
| --------------------- | ----------------------------------------------- | ------------ | -------------------------------------------------------------------- | ----- |
| Raspberry Pi 400      | 1.8 Hz, Quad-core Broadcom BCM2711 (Cortex-A72) | 4GB (LPDDR4) | 2 × USB 3.0, 1 x USB 2.0 ports, 2 x micro HDMI, 1 x Gigabit Ethernet | $70   |
| Raspberry Pi 4B       | 1.8 Hz, Quad-core Broadcom BCM2711 (Cortex-A72) | 8GB (LPDDR4) | 2x USB 3.0, 2x USB 2.0, 1x Gigabit Ethernet, 2x micro HDMI           | $75   |
| Raspberry Pi 4B       | 1.8 Hz, Quad-core Broadcom BCM2711 (Cortex-A72) | 4GB (LPDDR4) | 2x USB 3.0, 2x USB 2.0, 1x Gigabit Ethernet, 2x micro HDMI           | $55   |
| Raspberry Pi 4B       | 1.8 Hz, Quad-core Broadcom BCM2711 (Cortex-A72) | 2GB (LPDDR4) | 2x USB 3.0, 2x USB 2.0, 1x Gigabit Ethernet, 2x micro HDMI           | $35   |
| Raspberry Pi 3B+      | 1.4-GHz, 4-core Broadcom BCM2837B0 (Cortex-A53) | 1GB          | 4 x USB 2.0, HDMI, 3.5mm audio                                       | $35   |
| Raspberry Pi Zero WH  | 1-GHz, 1-core Broadcom BCM2835 (ARM1176JZF-S)   | 512MB        | 1x micro USB, 1x mini HDMI                                           | $17   |
| Raspberry Pi Zero W   | 1-GHz, 1-core Broadcom BCM2835 (ARM1176JZF-S)   | 512MB        | 1x micro USB, 1x mini HDMI                                           | $10   |
| Raspberry Pi Zero     | 1-GHz, 1-core Broadcom BCM2835 (ARM1176JZF-S)   | 512MB        | 1x micro USB, 1x mini HDMI                                           | $5    |
|                       |                                                 |              |                                                                      |       |

## Raspberry Pi OS

Raspberry Pi OS is a Debian-based [[Operating systems]] for Raspberry Pi. Since 2013, it has been officially provided by the Raspberry Pi Foundation as the primary operating system for the Raspberry Pi family of compact single-board computers.

## SSD aan een raspbarry pi hangen

In `cmdline.txt` moet je deze tekst plakken op het einde van de eerste regel:

```bash
usb-storage.quirks=152d:0578:u
```
