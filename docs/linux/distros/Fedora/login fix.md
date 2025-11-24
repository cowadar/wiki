---
title: Login fix
---

# GDM Login Scherm op Ultrawide Monitor - Fedora

## Het Probleem

Bij een multi-monitor setup met een ultrawide monitor verschijnt het GDM (GNOME Display Manager) login scherm vaak op het verkeerde scherm of is helemaal niet zichtbaar. Dit gebeurt omdat GDM zijn eigen monitor configuratie gebruikt, los van jouw gebruikersinstellingen.

## Wat we gaan doen

We kopiëren de monitor configuratie van jouw gebruikersaccount naar GDM, zodat het login scherm dezelfde instellingen gebruikt als jouw desktop. Hierdoor verschijnt het login scherm op de juiste monitor met de juiste resolutie.

## Oplossing

### Stap 1: Configureer je schermen

1. Log in op je systeem
2. Open **Settings** → **Displays**
3. Stel je ultrawide monitor in als **Primary** (primaire monitor)
4. Controleer of de positie en resolutie correct zijn
5. Klik op **Apply**

Dit maakt automatisch een `monitors.xml` bestand aan in `~/.config/`

### Stap 2: Kopieer de configuratie naar GDM

Open een terminal en voer deze commando's uit:

```bash
# Maak de directory aan als deze nog niet bestaat
sudo mkdir -p /etc/xdg

# Kopieer de monitor configuratie
sudo cp ~/.config/monitors.xml /etc/xdg/monitors.xml
```

### Stap 3: Herstart

```bash
sudo systemctl reboot
```

Na het herstarten zou je login scherm nu op de juiste monitor moeten verschijnen.

## Troubleshooting

### Het werkt nog steeds niet

Als het nog steeds niet werkt, controleer dan of het bestand correct is gekopieerd:

```bash
ls -l /etc/xdg/monitors.xml
```

Je zou iets moeten zien zoals:
```
-rw-r--r--. 1 root root 1234 Nov 24 10:30 /etc/xdg/monitors.xml
```

### Na een monitor wijziging

Als je later je monitor setup wijzigt, herhaal dan gewoon stap 1 en 2.

### Wayland vs X.Org

Deze oplossing werkt voor zowel Wayland als X.Org sessies op moderne Fedora versies.
