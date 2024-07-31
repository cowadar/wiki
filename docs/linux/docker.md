# Docker
## Docker Engine installeren op Ubuntu

Om aan de slag te gaan met Docker Engine op Ubuntu, moet u ervoor zorgen dat u [aan de vereisten voldoet](https://docs.docker.com/engine/install/ubuntu/#prerequisites) en vervolgens [Docker installeert](https://docs.docker.com/engine/install/ubuntu/#installation-methods) .

### Vereisten

#### OS-vereisten

Om Docker Engine te installeren, hebt u de 64-bits versie van een van deze Ubuntu-versies nodig:

- Ubuntu Jammy 22.04 (LTS)
- Ubuntu Impish 21.10
- Ubuntu Focal 20.04 (LTS)
- Ubuntu Bionic 18.04 (LTS)

Docker Engine wordt ondersteund op `x86_64`(of `amd64`), `armhf`, `arm64`, en `s390x`architecturen.

#### Oude versies verwijderen

Oudere versies van Docker werden `docker`, `docker.io`, of `docker-engine`. Als deze zijn geïnstalleerd, verwijder ze dan:

```bash
sudo apt-get remove docker docker-engine docker.io containerd runc
```

Het is oké als `apt-get`wordt gemeld dat geen van deze pakketten is geïnstalleerd.

De inhoud van `/var/lib/docker/`, inclusief afbeeldingen, containers, volumes en netwerken, blijft behouden. Als u uw bestaande gegevens niet hoeft op te slaan en met een schone installatie wilt beginnen, raadpleegt u het gedeelte [Docker Engine verwijderen](https://docs.docker.com/engine/install/ubuntu/#uninstall-docker-engine) onderaan deze pagina.

### Installatiemethoden [\_](https://docs.docker.com/engine/install/ubuntu/#installation-methods)

U kunt Docker Engine op verschillende manieren installeren, afhankelijk van uw behoeften:

- De meeste gebruikers [stellen de repositories van Docker in](https://docs.docker.com/engine/install/ubuntu/#install-using-the-repository) en installeren van daaruit, voor eenvoudige installatie en upgradetaken. Dit is de aanbevolen aanpak.

- Sommige gebruikers downloaden het DEB-pakket en [installeren het handmatig](https://docs.docker.com/engine/install/ubuntu/#install-from-a-package) en beheren upgrades volledig handmatig. Dit is handig in situaties zoals het installeren van Docker op air-gapped systemen zonder toegang tot internet.

- In test- en ontwikkelomgevingen kiezen sommige gebruikers ervoor om geautomatiseerde [gemaksscripts](https://docs.docker.com/engine/install/ubuntu/#install-using-the-convenience-script) te gebruiken om Docker te installeren.

#### Installeren met behulp van de repository

Voordat u Docker Engine voor de eerste keer op een nieuwe hostcomputer installeert, moet u de Docker-repository instellen. Daarna kunt u Docker installeren en bijwerken vanuit de repository.

##### De opslagplaats instellen

1. Werk de `apt`pakketindex bij en installeer pakketten om het `apt`gebruik van een repository via HTTPS toe te staan:

    ```bash
    $ sudo apt-get update

    $ sudo apt-get install \
        ca-certificates \
        curl \
        gnupg \
        lsb-release
    ```

2. Voeg de officiële GPG-sleutel van Docker toe:

    ```bash
    sudo mkdir -p /etc/apt/keyrings
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
    ```

3. Gebruik de volgende opdracht om de repository in te stellen:

    ```bash
    $ echo \
      "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
      $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
    ```

##### Docker Engine installeren

1. Werk de `apt`pakketindex bij en installeer de _nieuwste versie_ van Docker Engine, containerd en Docker Compose, of ga naar de volgende stap om een specifieke versie te installeren:

    ```bash
     sudo apt-get update
     sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin
    ```

2. Om een _specifieke versie_ van Docker Engine te installeren, vermeldt u de beschikbare versies in de repo, selecteert u en installeert u:

    a. Maak een lijst van de beschikbare versies in uw repo:

    ```bash
    $ apt-cache madison docker-ce

    docker-ce | 5:20.10.16~3-0~ubuntu-jammy | https://download.docker.com/linux/ubuntu jammy/stable amd64 Packages
    docker-ce | 5:20.10.15~3-0~ubuntu-jammy | https://download.docker.com/linux/ubuntu jammy/stable amd64 Packages
    docker-ce | 5:20.10.14~3-0~ubuntu-jammy | https://download.docker.com/linux/ubuntu jammy/stable amd64 Packages
    docker-ce | 5:20.10.13~3-0~ubuntu-jammy | https://download.docker.com/linux/ubuntu jammy/stable amd64 Packages
    ```

    b. Installeer een specifieke versie met behulp van de versiereeks uit de tweede kolom, bijvoorbeeld `5:20.10.16~3-0~ubuntu-jammy`.

    ```bash
    sudo apt-get install docker-ce=<VERSION_STRING> docker-ce-cli=<VERSION_STRING> containerd.io docker-compose-plugin
    ```

3. Controleer of Docker Engine correct is geïnstalleerd door de `hello-world` afbeelding uit te voeren.

    ```bash
    sudo service docker start
    sudo docker run hello-world
    ```

    Met deze opdracht wordt een testimage gedownload en uitgevoerd in een container. Wanneer de container wordt uitgevoerd, drukt deze een bericht af en wordt afgesloten.

Docker Engine is geïnstalleerd en draait. De `docker`groep is gemaakt, maar er worden geen gebruikers aan toegevoegd. U moet gebruiken `sudo`om Docker-opdrachten uit te voeren. Ga door naar [Linux na de installatie](https://docs.docker.com/engine/install/linux-postinstall/) om niet-bevoegde gebruikers toe te staan Docker-opdrachten uit te voeren en voor andere optionele configuratiestappen.

##### Docker-engine upgraden

Om Docker Engine te upgraden, voert u eerst uit `sudo apt-get update`, volgt u de [installatie-instructies](https://docs.docker.com/engine/install/ubuntu/#install-using-the-repository) en kiest u de nieuwe versie die u wilt installeren.

### Uninstall Docker Engine

1. Uninstall the Docker Engine, CLI, Containerd, and Docker Compose packages:

    ```bash
    sudo apt-get purge docker-ce docker-ce-cli containerd.io docker-compose-plugin
    ```

2. Images, containers, volumes, or customized configuration files on your host are not automatically removed. To delete all images, containers, and volumes:

    ```bash
    sudo rm -rf /var/lib/docker
    sudo rm -rf /var/lib/containerd
    ```

You must delete any edited configuration files manually.
