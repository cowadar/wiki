# Raspberry Pi Imager

## Wat is Raspberry Pi Imager?

Raspberry Pi Imager is een hulpprogramma waarmee je eenvoudig en snel een besturingssysteem naar een microSD-kaart kunt schrijven, klaar voor gebruik met je Raspberry Pi. Je kunt kiezen uit verschillende besturingssystemen die door Raspberry Pi worden aangeboden, zoals [Raspberry Pi OS](../linux/distros/rpi_os.md) en andere.

Raspberry Pi Imager downloadt het geselecteerde besturingssysteem rechtstreeks van de website van Raspberry Pi en schrijft het naar de SD-kaart. Dit versnelt het proces aanzienlijk in vergelijking met de standaardmethode van downloaden, opslaan en kopiëren. Raspberry Pi Imager is beschikbaar voor Windows, macOS en Ubuntu.

![rpi imager](https://assets.raspberrypi.com/static/md-bfd602be71b2c1099b91877aed3b41f0.png)


## Installatie

Om Raspberry Pi Imager te gebruiken, moet je het eerst downloaden en installeren op een computer met een SD-kaartlezer:

### Download

- Je kunt de [downloadlinks](https://www.raspberrypi.com/software/) vinden op de website van Raspberry Pi.
- Als je al Raspberry Pi OS of een [debian](../linux/distros/debian.md)-gebaseerd os gebruikt, kun je Raspberry Pi Imager ook installeren door `sudo apt install rpi-imager` te typen in een Terminal-venster.

Nadat je Raspberry Pi Imager hebt geïnstalleerd, stop je de SD-kaart die je wilt gebruiken met je Raspberry Pi in de lezer en start je Raspberry Pi Imager op.

1. Je ziet dan een eenvoudige interface met twee opties: **Choose OS** en **Choose SD Card**.
2. Klik op **Choose OS** om een lijst te zien van alle beschikbare besturingssystemen die je kunt installeren. Je kunt ook kiezen voor **Use custom** om een eigen image-bestand te selecteren.
3. Klik vervolgens op **Choose SD Card** om de SD-kaart te selecteren waarop je het besturingssysteem wilt schrijven. Zorg ervoor dat je de juiste kaart kiest, want alle gegevens erop zullen worden gewist
4. Klik ten slotte op **Write** om het schrijfproces te starten. Dit kan enkele minuten duren, afhankelijk van de grootte van het besturingssysteem en de snelheid van je SD-kaart.
5. Als het schrijfproces klaar is, kun je de SD-kaart uitwerpen en in je Raspberry Pi steken.
6. Start je Raspberry Pi op en volg de instructies op het scherm om je besturingssysteem in te stellen.
