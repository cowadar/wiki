# Pterodactyl

## Wat is pterodactyl
Pterodactyl is een open-source game server management paneel dat is gebouwd voor Docker-containers. Het stelt gebruikers in staat om game- en applicatieservers eenvoudig te beheren via een webinterface.
Belangrijke kenmerken van Pterodactyl:

- Webinterface: Intuïtief dashboard om servers te beheren.
- Docker-gebaseerd: Elke game server draait geïsoleerd in een container.
- Ondersteuning voor meerdere games: Werkt met Minecraft, CS:GO, Rust, ARK, FiveM en meer.
- Gebruikersbeheer: Mogelijkheid om meerdere gebruikers toegang te geven tot specifieke servers.
- API-integratie: Automatisering en integratie met andere systemen.
- Veiligheid: Runs als een niet-root gebruiker en gebruikt Wings als de daemon om servers te beheren.

Wil je Pterodactyl gebruiken om zelf game servers te hosten? 🚀

## Installatie

### Panel

Hieronder vind u de 2 docker compose Files:

??? "Configuratie bestanden"
    !!! warning
        Vergeet niet alle gegevens naar die van jou te veranderen!!
    !!! note
        https://github.com/pterodactyl/panel/blob/1.0-develop/docker-compose.example.yml
        https://github.com/pterodactyl/wings/blob/develop/docker-compose.example.yml
    === "pterodactyl-panel.yml"

        ```yaml
        x-common:
        database:
            &db-environment
            # Do not remove the "&db-password" from the end of the line below, it is important
            # for Panel functionality.
            MYSQL_PASSWORD: &db-password "MYSQL_PASSWORD"
            MYSQL_ROOT_PASSWORD: "$MYSQL_ROOT_PASSWORD"
        panel:
            &panel-environment
            APP_URL: "$APP_URL"
            # A list of valid timezones can be found here: http://php.net/manual/en/timezones.php
            APP_TIMEZONE: "$TZ"
            APP_SERVICE_AUTHOR: "$APP_SERVICE_AUTHOR"
            # Uncomment the line below and set to a non-empty value if you want to use Let's Encrypt
            # to generate an SSL certificate for the Panel.
            # LE_EMAIL: ""
        mail:
            &mail-environment
            MAIL_FROM: "noreply@example.com"
            MAIL_DRIVER: "smtp"
            MAIL_HOST: "mail"
            MAIL_PORT: "1025"
            MAIL_USERNAME: ""
            MAIL_PASSWORD: ""
            MAIL_ENCRYPTION: "true"

        #
        # ------------------------------------------------------------------------------------------
        # DANGER ZONE BELOW
        #
        # The remainder of this file likely does not need to be changed. Please only make modifications
        # below if you understand what you are doing.
        #
        services:
        database:
            image: mariadb:10.5
            container_name: maridb
            restart: always
            command: --default-authentication-plugin=mysql_native_password
            volumes:
            - "$PTERODACTYLDIR/panel/database:/var/lib/mysql"
            environment:
            <<: *db-environment
            MYSQL_DATABASE: "$MYSQL_DATABASE"
            MYSQL_USER: "$MYSQL_USER"
        cache:
            image: redis:alpine
            container_name: cache
            restart: always
        panel:
            image: ghcr.io/pterodactyl/panel:latest
            container_name: panel
            restart: always
            ports:
            - "${PTERODACTYLPANELPORTS_HTTP:-80}:80"
            - "${PTERODACTYLPANELPORTS_HTTPS:-443}:443"
            links:
            - database
            - cache
            volumes:
            - "$PTERODACTYLDIR/panel/var/:/app/var/"
            - "$PTERODACTYLDIR/panel/nginx/:/etc/nginx/http.d/"
            - "$PTERODACTYLDIR/panel/certs/:/etc/letsencrypt/"
            - "$PTERODACTYLDIR/panel/logs/:/app/storage/logs"
            environment:
            <<: [*panel-environment, *mail-environment]
            DB_PASSWORD: *db-password
            APP_ENV: "$APP_ENV"
            APP_ENVIRONMENT_ONLY: "false"
            CACHE_DRIVER: "redis"
            SESSION_DRIVER: "redis"
            QUEUE_DRIVER: "redis"
            REDIS_HOST: "cache"
            DB_HOST: "database"
            DB_PORT: "3306"
        networks:
        default:
            ipam:
            config:
                - subnet: 172.20.0.0/16
        ```
    === "pterodactyl-wings.yml"

        ```yaml
        services:
        wings:
            image: ghcr.io/pterodactyl/wings:latest
            container_name: wing1
            restart: always
            networks:
            - wings0
            ports:
            - "${PTERODACTYLNODE1_PORTS_HTTP:-8080}:8080"
            - "${PTERODACTYLNODE1_PORTS_FTP:-2022}:2022"
            - "${PTERODACTYLNODE1_PORTS_HTTPS:-444}:443"
            tty: true
            environment:
            TZ: "$TZ"
            WINGS_UID: 988
            WINGS_GID: 988
            WINGS_USERNAME: $WINGS_USERNAME
            volumes:
            - "/var/run/docker.sock:/var/run/docker.sock"
            - "/var/lib/docker/containers/:/var/lib/docker/containers/"
            - "$PTERODACTYLDIR/node1/etc/pterodactyl/:/etc/pterodactyl/"
            - "$PTERODACTYLDIR/node1/var/lib/pterodactyl/:/var/lib/pterodactyl/"
            - "$PTERODACTYLDIR/node1/var/log/pterodactyl/:/var/log/pterodactyl/"
            - "$PTERODACTYLDIR/node1/tmp/pterodactyl/:/tmp/pterodactyl/"
            - "$PTERODACTYLDIR/node1/etc/ssl/certs:/etc/ssl/certs:ro"
            # you may need /srv/daemon-data if you are upgrading from an old daemon
            #- "/srv/daemon-data/:/srv/daemon-data/"
            # Required for ssl if you use let's encrypt. uncomment to use.
            #- "/etc/letsencrypt/:/etc/letsencrypt/"

        networks:
        wings0:
            name: wings0
            driver: bridge
            ipam:
            config:
                - subnet: "172.21.0.0/16"
            driver_opts:
            com.docker.network.bridge.name: wings0
        ```
    === ".env"

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
        DOMAINNAME_1=DOMAIN # Domeinnaam
        HOSTNAME=Hostname # Hostname
        USERNAME=<USER>
        PASSWORD=<PASSWORD>



        ###### MYSQL #####
        MYSQL_PASSWORD=PASSWORD
        MYSQL_ROOT_PASSWORD=PASSWORD
        MYSQL_DATABASE=panel
        MYSQL_USER=pterodactyl

        ###### Pterodactyl ######
        APP_URL=https://panel.$DOMAINNAME_1
        APP_SERVICE_AUTHOR=EMAIL
        PTERODACTYLDIR=/pterodactyl
        APP_ENV=production


        ###### NODE1 ######
        WINGS_USERNAME=USERNAME



        ###### PORTS #######
        PTERODACTYLPANELPORTS_HTTP=80
        PTERODACTYLPANELPORTS_HTTPs=443
        PTERODACTYLNODE1_PORTS_HTTP=8080
        PTERODACTYLNODE1_PORTS_FTP=2022
        PTERODACTYLNODE1_PORTS_HTTPS=444
        ``` 

na dat je dit gedaan hebt doe je:
```bash
docker compose up -d
```

Nu ga je naar uw DNS provider en maak je een Record aan panel.domain.com --> WAN IP.
Doe dit hetzelfde voor de WING.
!!! note
    Als je local DNS kunt doen mag dit voor de Wing.
    Pterodactyl is gemaakt voor meerdere nodes in verschillende datacenters.

Als je eenmaal aan je Panel kunt voor je volgende commando uit um een user te maken:
```bash
docker-compose run --rm panel php artisan p:user:make
```

## Configuratie

### Treafik
Eerst gaan we de reverse proxy instellen.
Maak 2 URLs:
- panel.DOMAIN.COM
- node.DOMAIN.COM

!!! note
    Hou er rekening mee dat je een midelware moete instellen anders ga je Corsall errors krijgen.

    ```yaml
    http:
        middlewares:
            cors-pterodactyl:
            headers:
                accessControlAllowMethods:
                - "OPTIONS"
                - "POST"
                - "GET"
                - "PUT"
                - "DELETE"
                accessControlAllowHeaders:
                # - "*" # If you do this you get errors for "*"
                - "Accept"
                - "Authorization"
                - "Cache-Control"
                - "Content-Type"
                - "DNT"
                - "If-Modified-Since"
                - "Keep-Alive"
                - "Origin"
                - "User-Agent"
                - "X-Requested-With"
                accessControlAllowOriginList:
                - "*"
                accessControlMaxAge: 100
                addVaryHeader: true
                customRequestHeaders:
                X-Forwarded-Proto: "https"
                Content-Type: "application/json"
                customResponseHeaders:
                X-Forwarded-Proto: "https"
    ```


### Panel configuration
Als alles goed is ingesteld ga je naar je Panel.
Dan Stel je uw location is.
Als dat gadaan is stel je uw Node in (dit is het moeilijkste).
Tijdens het aanmaken van de node verwander je 8080 --> 443 (voor https connecties).
Vergeet zeten geen IP en poort allocatie in deze setup op te zetten. Dit zijn de poorten dat je Node kan gebruiken voor servers.

### node
Nu krijg je een config file dat je op de node moet zetten.
Onder "/etc/pterodactyl/config.yml"
!!! note    
    Vrander in deze file de 443 --> 8080.
    Dit komt omdat we hierboven moeten verwijzen naar HTTPS maar onde Deamon werkt op 8080.

!!! warning 
    Probeer eerst je panel url te laten staan.
    Word je verbinding (Hartslag) niet opgezet verander dit in het lokaal op van je Panel.

### Testen
Probeer nu een server op te zetten en alles Werkt
