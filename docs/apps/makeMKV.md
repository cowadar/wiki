# MakeMKV

MakeMKV is een programma dat je kunt gebruiken om video’s die je bezit om te zetten naar een gratis en patentvrij formaat dat overal kan worden afgespeeld. MakeMKV is een formaatconverter, ook wel “transcoder” genoemd. Het converteert videoclips van een eigen (en meestal versleutelde) schijf naar een set MKV-bestanden, waarbij de meeste informatie behouden blijft maar niet wordt gewijzigd. Het MKV-formaat kan meerdere video-/audiotracks met alle meta-informatie opslaan en hoofdstukken behouden. Er zijn veel spelers die MKV-bestanden op bijna alle platforms kunnen afspelen en er zijn hulpmiddelen om MKV-bestanden om te zetten naar vele formaten, waaronder DVD- en Blu-ray-schijven. Bovendien kan MakeMKV direct gedecodeerde video streamen zonder tussenconversie naar een breed scala aan spelers, zodat je Blu-ray- en DVD-schijven kunt bekijken met je favoriete speler op je favoriete besturingssysteem of op je favoriete apparaat.

## Koppel je DVD station
### Unraid
ALs je met unraid werkt kan je eenvoudig je DVD/Blue-ray speler koppelen aan je docker container.
Dit doe je door eerst op te zoeken welke je DVD/Blue-ray station is.
```bash
lsscsi
```
Dan krijg je volgende output: `cd/dvd  TSSTcorp DVDWBD SH-B123L  SB02  /dev/sr0   /dev/sg5` .
Hier zie je dat `/dev/sr0` je DVD station is. nu moet je nog zien welke 

Doe vervolgens in de `/dev` folder
```bash
ls-l
```
Dan krijg je een lijst en zoek je acheter `crw-rw---- 1 root cdrom    21,     3 Jun 13 14:23 sg3`.
Nu zie je dat `/dev/sg3/` je CD rom station is.

Als laatste voeg je aan je container volgende parameter toe bij `Extra Parameters:``--device /dev/sr0 --device /dev/sg0`

### Proxmox
Voor Proxmox zijn de stappen ongeveer hetzelfde.
Maar eerste voer je volgende stappen uit op de Proxmox host.
```bash
lsscsi -g
```
Dan krijg je volgende output: `cd/dvd  TSSTcorp DVDWBD SH-B123L  SB02  /dev/sr0   /dev/sg5` .
Hier zie je dat `/dev/sr0` je DVD station is. En hier zie je dat `/dev/sg5/` je CD rom station is.

Nu maak je eerst 2 *Device passtrough* aan in de LXC met de waardes van hier boven.
Dit kan zowel via de GUI als via de cli van Proxmox
```
nano /etc/pve/lxc/101.conf
dev0: /dev/sr0,gid=0,mode=0666,uid=0
dev1: /dev/sg5,gid=0,mode=0666,uid=0
```

Voege deze nu toe in onderstaande docker compose.

```yaml
services:
    makemkv:
        container_name: makemkv
        ports:
            - 5800:5800
        volumes:
            - /docker/appdata/makemkv:/config:rw
            - /home/user:/storage:ro
            - /home/user/MakeMKV/output:/output:rw
        devices:
            - "/dev/sr0:/dev/sr0"
            - "/dev/sg5:/dev/sg5"
        image: jlesage/makemkv
```