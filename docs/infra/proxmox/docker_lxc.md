# Installatie van Docker in een LXC container (turnkey-core) op Proxmox

Volg de stappen hieronder om Docker te installeren in een LXC container (turnkey-core) op Proxmox.

## Stap 1: Download het turnkey-core sjabloon

De eerste stap is om het turnkey-core sjabloon te downloaden naar je opslag.

- Klik op je opslag en klik vervolgens op de knop **Templates**.
- Zoek naar het core sjabloon en klik op **Download**.
- Wacht tot het downloaden is voltooid.

## Stap 2: Maak een nieuwe LXC container

De tweede stap is om een nieuwe LXC container te maken met behulp van het turnkey-core sjabloon.

- Klik op de knop **Create CT** in de rechterbovenhoek.
- Voer een **Hostname** in, bijvoorbeeld `docker`.
- Voer het **Password** in dat je wilt gebruiken. Dit wachtwoord wordt gebruikt om in te loggen op de root gebruikersaccount. Nadat alle instellingen zijn opgegeven, klik je op **Next**.
- Selecteer het **Template**, bijvoorbeeld `local:vztmpl/turnkey-core-16.1-buster-amd64.tar.gz`, en klik op **Next**.
- Selecteer de **Disk Size** voor deze container, bijvoorbeeld `8 GB`, en klik op **Next**.
- Selecteer het totale aantal **Cores** voor de CPU, bijvoorbeeld `2`, en klik op **Next**.
- Stel het totale **Memory** in, bijvoorbeeld `2 GB`, en klik op **Next**.
> Let op: dit is de limiet die deze container kan gebruiken.
- Wijzig het **Network** naar DHCP voor IPv4 en IPv6 (tenzij je ze handmatig wilt opgeven), en klik op **Next** tot je bij **Confirm** komt.
> Let op: we slaan het DNS-gedeelte over, maar je kunt deze instellingen wijzigen als je een andere DNS-server wilt gebruiken dan de Proxmox host.
- Bevestig de instellingen en klik op **Finish** om de container te maken!

## Stap 3: Start de LXC container en log in

De derde stap is om de LXC container te starten en in te loggen met behulp van ssh.

- Selecteer de LXC container die we zojuist hebben gemaakt en klik op **Start**.
- Klik vervolgens op **Console** om toegang te krijgen tot de terminal van de container.
- Log in met de gebruikersnaam `root` en het wachtwoord dat je in stap twee hebt ingesteld.
- Je wordt gevraagd om de appliance te initialiseren. Volg de instructies op het scherm om akkoord te gaan met de licentieovereenkomst, een nieuw root wachtwoord in te stellen, een domeinnaam in te voeren, een API-sleutel voor TurnKey Hub over te slaan en beveiligingsupdates toe te staan.

## Stap 4: Installeer Docker in de LXC container

De vierde stap is om Docker te installeren in de LXC container.

- Voer het volgende commando uit om het systeem bij te werken:

```bash
apt update && apt upgrade -y
```

- Voer vervolgens het volgende commando uit om Docker te installeren:

```bash
curl -fsSL https://get.docker.com | sh
```

- Voeg de root gebruiker toe aan de docker groep met het volgende commando:

```bash
usermod -aG docker root
```

- Start de docker service met het volgende commando:

```bash
systemctl start docker
```

- Controleer of docker correct werkt met het volgende commando:

```bash
docker run hello-world
```

Je zou een bericht moeten zien dat bevestigt dat Docker succesvol is geïnstalleerd en dat je een container kunt uitvoeren.

Gefeliciteerd, je hebt Docker geïnstalleerd in een LXC container (turnkey-core) op Proxmox!

## Links

- [How to Set Up Docker Containers in Proxmox - WunderTech](https://www.wundertech.net/how-to-set-up-docker-containers-in-proxmox/)
- [Docker | TurnKey GNU/Linux](https://www.turnkeylinux.org/docs/docker)
- [Setup and Install Docker in a Proxmox LXC Conainer | The Homelab Wiki](https://thehomelab.wiki/books/promox-ve/page/setup-and-install-docker-in-a-promox-lxc-conainer)
