# MakeMKV

MakeMKV is een programma dat je kunt gebruiken om video’s die je bezit om te zetten naar een gratis en patentvrij formaat dat overal kan worden afgespeeld. MakeMKV is een formaatconverter, ook wel “transcoder” genoemd. Het converteert videoclips van een eigen (en meestal versleutelde) schijf naar een set MKV-bestanden, waarbij de meeste informatie behouden blijft maar niet wordt gewijzigd. Het MKV-formaat kan meerdere video-/audiotracks met alle meta-informatie opslaan en hoofdstukken behouden. Er zijn veel spelers die MKV-bestanden op bijna alle platforms kunnen afspelen en er zijn hulpmiddelen om MKV-bestanden om te zetten naar vele formaten, waaronder DVD- en Blu-ray-schijven. Bovendien kan MakeMKV direct gedecodeerde video streamen zonder tussenconversie naar een breed scala aan spelers, zodat je Blu-ray- en DVD-schijven kunt bekijken met je favoriete speler op je favoriete besturingssysteem of op je favoriete apparaat.

## Koppel je DVD station

ALs je met unraid werkt kan je eenvoudig je DVD/Blue-ray speler koppelen aan je docker container.
Dit doe je door eerst op te zoeken welke je DVD/Blue-ray station is.
```bash
    lsscsi
```
Dan krijg je volgende output: `cd/dvd  TSSTcorp DVDWBD SH-B123L  SB02  /dev/sr0` .
Hier zie je dat `/dev/sr0` je DVD station is. nu moet je nog zien welke 

Doe vervolgens in de `/dev` folder
```bash
    ls-l
```
Dan krijg je een lijst en zoek je acheter `crw-rw---- 1 root cdrom    21,     3 Jun 13 14:23 sg3`.
Nu zie je dat `/dev/sg3/` je CD rom station is.

Als laatste voeg je aan je container volgende parameter toe bij `Extra Parameters:``--device /dev/sr0 --device /dev/sg0`