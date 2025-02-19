# Docker-compose

Docker-compose is een software die wordt gebruikt om multi-container Docker applicaties te definiëren en uit te voeren. Het kan meerdere containers tegelijkertijd afhandelen in de productie-, staging-, ontwikkelings-, test- en CI-omgeving. Gebruik daarom docker-compose om de hele softwareontwikkelingscyclus (SDLC) te beheren.

Met docker-compose kun je een YAML-bestand maken om de services van je applicatie te configureren. Vervolgens kun je met één commando alle services uit je configuratie maken en starten. Het grote voordeel van docker-compose is dat je je applicatiestack in een bestand kunt definiëren, bij de root van je projectrepo kunt bewaren (het is nu versiebeheerd) en gemakkelijk iemand anders kunt laten bijdragen aan je project.

![logo](https://www.suse.com/c/wp-content/uploads/2021/09/rancher_blog_compose.png)


## Networking
Docker-Compose maakt standaard een nieuw netwerk aan voor het gegeven compose-bestand. U kunt het gedrag wijzigen door aangepaste netwerken te definiëren in uw samengestelde bestand.

### Aangepast netwerk maken en toewijzen

```yaml
networks:
  custom-network:

services:
  app:
    networks:
      - custom-network
```
### Gebruik bestaande netwerken
Als u een bestaand Docker-netwerk wilt gebruiken voor uw samengestelde bestanden, kunt u de `external: true` parameter toevoegen aan uw samengestelde bestand

```yaml
networks:
  existing-network:
    external: true
```

## Volumes
Met volumes kunnen Docker-containers permanente opslag gebruiken. In een samengesteld bestand kunt u volumes als volgt maken en toewijzen:
```yaml
volumes:
  my-volume:

services:
  app:
    volumes:
      - my-volume:/path-in-container
```

Deze volumes worden vaak opgeslagen in `/var/lib/docker/volumes`.


##  Docker compose (Easy)

Docker Compose is een tool waarmee je meerdere Docker-containers kunt definiëren en beheren via een docker-compose.yml-bestand. Hiermee kun je eenvoudig complete applicaties met meerdere services (zoals databases, backends en frontends) opstarten met één commando.

🔹 Belangrijkste functies:

- Definieert containers en hun configuratie in YAML.
- Start alles met "docker compose up".
- Beheert netwerken, volumes en afhankelijkheden automatisch.

🚀 Kort gezegd: Docker Compose maakt het eenvoudiger om multi-container applicaties te beheren!


### voobeeld
laten we een opstelling maken om het te verduidelijken.
maak deze struktuur.

```bash
docker
  └── docker-compose.yaml
```

Ze het volgend in de "docker-compose.yaml"
```yaml 
services:
  mariadb:
    image: lscr.io/linuxserver/mariadb:latest
    container_name: mariadb
    networks:
      - default
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Etc/UTC
      - MYSQL_ROOT_PASSWORD=ROOT_ACCESS_PASSWORD
      - MYSQL_DATABASE=USER_DB_NAME #optional
      - MYSQL_USER=MYSQL_USER #optional
      - MYSQL_PASSWORD=DATABASE_PASSWORD #optional
      - REMOTE_SQL=http://URL1/your.sql,https://URL2/your.sql #optional
    volumes:
      - ./mariadb/config:/config
    ports:
      - 3306:3306
    restart: unless-stopped

  phpmyadmin:
    image: phpmyadmin
    restart: always
    networks:
      - default
    ports:
      - 8080:80
    environment:
      - PMA_ARBITRARY=1
```

Als je dit gedaan hebt doe je het volgende:

```bash
cd ~/docker
docker compose up -d
```

Nu zal zullen je dockers opgestart worden. Je kan hier zoveel docker insteken.

### update compose

Als je nu heel simpel je dockers wilt updaten doe je het volgende:
```bash
docker compose pull
docker compose up -d
```

##  Docker compose (Advanced)

Docker Compose ondersteunt variabelen en externe opslag voor flexibiliteit en betere configuratiebeheer.

🔹 Belangrijkste functies:

    Variabelen gebruiken via een .env-bestand of direct in docker-compose.yml (bijvoorbeeld ${VARIABELE} voor dynamische configuratie).
    Opslag op andere plaatsen door volumes of bind mounts te gebruiken, zodat data persistent blijft buiten de container.

🚀 Kort gezegd: Docker Compose maakt het eenvoudig om multi-container applicaties te beheren, inclusief dynamische configuratie en externe opslag!

### voobeeld
laten we een opstelling maken om het te verduidelijken.
maak deze struktuur.

```bash
docker
  ├── appdata
  └── docker-compose
        ├──compose
        │   └── database.yml
        ├──docker-compose.yaml
        └──.env
        
```

Ze het volgend in de "docker-compose.yaml"

??? "Configuratie bestanden"
    !!! warning
        Vergeet niet alle gegevens naar die van jou te veranderen!!
    === "docker-compose.yaml"

        ```yaml
            ########################### NETWORKS ######################
            # U kunt de onderstaande netwerk-subnetten (10.250.x.0/24) naar wens aanpassen.
            networks:
              default:
                driver: bridge
                ipam:
                  config:
                    - subnet: 10.250.10.0/24
            include:
              ########################### SERVICES ####################
              - compose/database.yml
        ```
    === "database.yml"

        ```yaml
        services:
          mariadb:
            image: lscr.io/linuxserver/mariadb:latest
            container_name: mariadb
            networks:
              - default
            environment:
              - PUID=$PUID
              - PGID=$PGID
              - TZ=$TZ
              - MYSQL_ROOT_PASSWORD=$ROOTPASSWORD
              - MYSQL_DATABASE=$USER_DB_NAME #optional
              - MYSQL_USER=$MYSQL_USER #optional
              - MYSQL_PASSWORD=$DATABASE_PASSWORD #optional
              - REMOTE_SQL=http://db.$DOMAINNAME_1/your.sql,https://db2.$DOMAINNAME_1/your.sql #optional
            volumes:
              - $APPDATADIR/mariadb/config:/config
            ports:
              - ${MARIADB_PORT:-3306}:3306
            restart: unless-stopped

          phpmyadmin:
            image: phpmyadmin
            restart: always
            networks:
              - default
            ports:
              - ${PHPMYADMIN_PORT:-8080}:80
            environment:
              - PMA_ARBITRARY=1
        ```

    === ".env"
        !!! note
            Dit bestand is voorzien om al je veriabele in te zetten.

        ```yaml
          PUID=1000 # User ID
          PGID=1003 #Group ID
          UMASK=002
          TZ=Europe/Brussels # Timezone
          USERDIR=/home/<USER> # User directory
          DOCKERDIR=/home/<USER>/docker # Docker directory
          COMPOSEDIR=/home/<USER>/docker/docker-compose/compose # Compose directory
          APPDATADIR=/home/<USER>/docker/appdata # Appdata directory
          DATADIR=/data
          LOCAL_IPS=127.0.0.1/32,10.0.0.0/8,192.168.0.0/16,172.16.0.0/16 # Lokale IP's
          CLOUDFLARE_IPS=173.245.48.0/20,103.21.244.0/22,103.22.200.0/22,103.31.4.0/22,141.101.64.0/18,108.162.192.0/18,190.93.240.0/20,188.114.96.0/20,197.234.240.0/22,198.41.128.0/17,162.158.0.0/15,104.16.0.0/13,104.24.0.0/14,172.64.0.0/13,131.0.72.0/22 # Cloudflare IP's
          DOMAINNAME_1=<DOMAIN.COM> # Domeinnaam
          HOSTNAME=<HOSTNAME> # Hostname
          USERNAME=<USER>
          PASSWORD=<PASSWORD>

          ###### DATABASE #####
          ROOTPASSWORD=<ROOTPASSWORD>
          USER_DB_NAME=<USER_DB_NAME>
          MYSQL_USER=<MYSQL_USER>
          DATABASE_PASSWORD=<DATABASE_PASSWORD>



          ###### PORTS #####
          MARIADB_PORT=3306
          PHPMYADMIN_PORT=8080
        ```

