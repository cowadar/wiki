# Treafik

## Certificaten voor meerdere domeinen in Traefik

Zie [https://doc.traefik.io/traefik/routing/routers/#certresolver](https://doc.traefik.io/traefik/routing/routers/#certresolver)


## Treafik

![Voorbeeld cmd](../../_assets/images/traefik-reverse-proxy.png)

Traefik is een reverse proxy. Dit wil zeggen dat je heel gemakkelijk verschillende services kunt bereiken vanaf het internet zonder elke keer een port forward te doen.
De enige poort(en) die je moet openzetten is 443 (en 80). En verwijzen naar je traefik IP.

In dit voorbeeld gebruiken we Unifi
![Voorbeeld cmd](../../_assets/images/traefik_port_forwarding.png)

Bij je DNS provider verwijs je een A record door naar je Publiek IP (dit kan ook automatisch gebeuren door [DDNS](ddns.md)).
Voor alle services gebruik je een CNAME record die verwijst naar @ (@ = jouw domein).
![Voorbeeld cmd](../../_assets/images/traefik_a_record.png)
![Voorbeeld cmd](../../_assets/images/traefik_cname_record.png)


### Installatie bestanden
Als je dat eenmaal gedaan hebt kan je beginnen aan de configuratie van Traefik.

!!! info 
    Onze Traefik gebruikt een dynamische file. Dat wilt zeggen dat je als je de config aanpast dat je niet elke keer de docker opnieuw moet genereren.

!!! info
    Graag volgens volgende folder structuur maken.

    ```bash
    docker
    ├── appdate
    │   └── traefik
    │       └── logs 
    │           ├── access.log
    │           └── traefik.log
    └── docker-compose
        └── traefik
            ├── data
            │    ├── acme.json
            │    ├── config.yml
            │    └── traefik.yml
            ├── .env
            ├── cf_api_token.txt
            └── docker-compose.yaml

    ```

??? "Config files"
    !!! warning
        Vergeet niet alle gegevens naar die van jou te veranderen!!
    === "docker-compose.yaml"

        ```yaml
        version: "3.8"

        services:
          traefik:
            image: traefik:v3.0
            container_name: traefik
            restart: unless-stopped
            security_opt:
              - no-new-privileges:true
            networks:
              - proxy
            ports:
              - 80:80
              - 443:443/tcp
              # - 443:443/udp # Uncomment if you want HTTP3
            environment:
              CF_DNS_API_TOKEN_FILE: /run/secrets/cf_api_token # note using _FILE for docker secrets
              # CF_DNS_API_TOKEN: ${CF_DNS_API_TOKEN} # if using .env
              TRAEFIK_DASHBOARD_CREDENTIALS: ${TRAEFIK_DASHBOARD_CREDENTIALS}
            secrets:
              - cf_api_token
            env_file: .env # use .env
            volumes:
              - /etc/localtime:/etc/localtime:ro
              - /var/run/docker.sock:/var/run/docker.sock:ro
              - ./data/traefik.yml:/traefik.yml:ro
              - ./data/acme.json:/acme.json
              - ./data/config.yml:/config.yml:ro
              - /home/USER/appdata/traefik/logs:/var/log/traefik
            labels:
              - "traefik.enable=true"
              - "traefik.http.routers.traefik.entrypoints=http"
              - "traefik.http.routers.traefik.rule=Host(`traefik-dashboard.domain.be`)" # Aanpassen
              - "traefik.http.middlewares.traefik-auth.basicauth.users=${TRAEFIK_DASHBOARD_CREDENTIALS}"
              - "traefik.http.middlewares.traefik-https-redirect.redirectscheme.scheme=https"
              - "traefik.http.middlewares.sslheader.headers.customrequestheaders.X-Forwarded-Proto=https"
              - "traefik.http.routers.traefik.middlewares=traefik-https-redirect"
              - "traefik.http.routers.traefik-secure.entrypoints=https"
              - "traefik.http.routers.traefik-secure.rule=Host(`traefik-dashboard.domain.be`)" #Aanpassen 
              - "traefik.http.routers.traefik-secure.middlewares=traefik-auth"
              - "traefik.http.routers.traefik-secure.tls=true"
              - "traefik.http.routers.traefik-secure.tls.certresolver=cloudflare"
              - "traefik.http.routers.traefik-secure.tls.domains[0].main=domain.be" #Aanpassen # copy past indien meerdere domeinen maar dan met een 1 (Array)
              - "traefik.http.routers.traefik-secure.tls.domains[0].sans=*.domain.be" #Aanpasen # copy past indien meerdere domeinen maar dan met een 1 (Array)
              - "traefik.http.routers.traefik-secure.service=api@internal"

        secrets:
          cf_api_token:
            file: ./cf_api_token.txt

        networks:
          proxy:
            external: true
        ```
    === "cf_api_token.txt"

        ```yaml
           #api token cloudflare
           #Cloudflare --> Right ebove user --> Appearance --> api tokens --> create token
           #Chose 1 or more domains
           #Plain tekst below
        ```
        ![Voorbeeld cmd](../../_assets/images/traefik_user_api_token.png)
    === ".env"

        ``` yaml
        # Declaring the user list
        #
        # Note: when used in docker-compose.yml all dollar signs in the hash need to be doubled for escaping.
        # To create a user:password pair, the following command can be used:
        # echo $(htpasswd -nb user password) | sed -e s/\\$/\\$\\$/g
        #
        # Also note that dollar signs should NOT be doubled when they not evaluated (e.g. Ansible docker_container module).
        TRAEFIK_DASHBOARD_CREDENTIALS=admin:$$2y$$05$$8eA6bz6E7J/ChsRFuD8njeW45yfJutYYb4HxwgUir3HP4EsggP/QNo0.
        ```
    === "traefik.yml"

        !!! tip
            Bent u in test fase? Zet dan de "caServer: https://acme-staging-v02.api.letsencrypt.org/directory" op staging.
            Lijn 60 aan en lijn 59 uit. Als je dit niet doet kan je geblokkeerd worden als je te veel request doet.
            Een request gebeurd als je traefik opnieuw start.

        ``` yaml
        api:
          dashboard: true
          debug: true

        entryPoints:
          http:
            address: ":80"
            forwardedHeaders:
              trustedIPs: &trustedIps
                # Start of Clouflare public IP list for HTTP requests, remove this if you don't use it
                - 173.245.48.0/20
                - 103.21.244.0/22
                - 103.22.200.0/22
                - 103.31.4.0/22
                - 141.101.64.0/18
                - 108.162.192.0/18
                - 190.93.240.0/20
                - 188.114.96.0/20
                - 197.234.240.0/22
                - 198.41.128.0/17
                - 162.158.0.0/15
                - 104.16.0.0/13
                - 104.24.0.0/14
                - 172.64.0.0/13
                - 131.0.72.0/22
                - 2400:cb00::/32
                - 2606:4700::/32
                - 2803:f800::/32
                - 2405:b500::/32
                - 2405:8100::/32
                - 2a06:98c0::/29
                - 2c0f:f248::/32
                # End of Cloudlare public IP list
            http:

              redirections:
                entryPoint:
                  to: https
                  scheme: https
          https:
            address: ":443"

        serversTransport:
          insecureSkipVerify: true

        providers:
          docker:
            endpoint: "unix:///var/run/docker.sock"
            exposedByDefault: false
          file:
            filename: /config.yml
            watch: true

        certificatesResolvers:
          cloudflare:
            acme:
              email: user@domain.be
              storage: acme.json
              caServer: https://acme-v02.api.letsencrypt.org/directory # prod (default)
              # caServer: https://acme-staging-v02.api.letsencrypt.org/directory # staging
              dnsChallenge:
                provider: cloudflare
                #disablePropagationCheck: true # uncomment this if you have issues pulling certificates through cloudflare, By setting this flag to true disables the need to wait for the propagation of the TXT record to all authoritative name servers.
                #delayBeforeCheck: 60s # uncomment along with disablePropagationCheck if needed to ensure the TXT record is ready before verification is attempted
                resolvers:
                  - "1.1.1.1:53"
                  - "1.0.0.1:53"

        log:
          level: "INFO"
          filePath: "/var/log/traefik/traefik.log"
        accessLog:
          filePath: "/var/log/traefik/access.log"
        ```   

    === "config.yml"

        ```yaml
        http:
        #region routers
          routers:
            sub:
              entryPoints:
                - "https"
              rule: "Host(`sub.domain.be`)"
              middlewares:                      # Desable This if you having troubles 
                - default-headers               # Learn more about Middlewares
                - https-redirectscheme          #
              tls: {}
              service: sub


        #endregion
        #region services
          services:
            sub:
              loadBalancer:
                servers:
                  - url: "http://172.30.0.50:5042"
                passHostHeader: true




        #endregion
          middlewares:
            https-redirectscheme:
              redirectScheme:
                scheme: https
                permanent: true

            default-headers:
              headers:
                frameDeny: true
                browserXssFilter: true
                contentTypeNosniff: true
                forceSTSHeader: true
                stsIncludeSubdomains: true
                stsPreload: true
                stsSeconds: 15552000
                customFrameOptionsValue: SAMEORIGIN
                customRequestHeaders:
                  X-Forwarded-Proto: https

            default-whitelist:
              ipWhiteList:
                sourceRange:
                - "10.0.0.0/8"
                - "192.168.0.0/16"
                - "172.16.0.0/12"

            secured:
              chain:
                middlewares:
                - default-whitelist
                - default-headers
        ```
    === "acme.json"
          
        !!! warning
            Deze file laat je leeg deze word zelf aangevult.


### Run
Als je alle bovenstaande stappen gedaan hebt voer je het volgende commado in de terminal in dezelfde folder als docker compose.
```bash
docker compose up -d
``` 

!!! warning
    Check de acme.json deze zou ingevuld moeten worden en daar zie je de certificaten.

### Configureer traefik.yaml

Zoals eerder gezegt is deze file dynamisch. copy past de code voor elke (sub)domein.


??? "traefik.yml edit"

    ```yaml
    routers:
                sub:
                entryPoints:
                    - "https"
                rule: "Host(`sub.domain.be`)"
                middlewares:                      # Desable This if you having troubles 
                    - default-headers               # Learn more about Middlewares
                    - https-redirectscheme          #
                tls: {}
                service: sub

                sub2:
                entryPoints:
                    - "https"
                rule: "Host(`sub2.domain.be`)"
                middlewares:                       
                    - default-headers               
                    - https-redirectscheme          
                tls: {}
                service: sub2


            #endregion
            #region services
            services:
                sub:
                loadBalancer:
                    servers:
                    - url: "http://172.30.0.50:5042"
                    passHostHeader: true

                sub2:
                loadBalancer:
                    servers:
                    - url: "http://172.30.0.51:5042"
                    passHostHeader: true
    ```
### Advanced
U Kunt ook traefik zijn configuratie automatisch laten verlopen door middel van labels op je docker containers te zetten.
De configuratie bij uw DNS provider zal nog steeds handmatig moeten gebeuren.

??? Labels

    ```yaml
        labels:
        # if you are not using traefik, comment out labels
        - "traefik.enable=true"
        - "traefik.http.routers.portainer.entrypoints=http"
        - "traefik.http.routers.portainer.rule=Host(`pihole.domain.be`)"
        - "traefik.http.middlewares.portainer-https-redirect.redirectscheme.scheme=https"
        - "traefik.http.routers.portainer.middlewares=portainer-https-redirect"
        - "traefik.http.routers.portainer-secure.entrypoints=https"
        - "traefik.http.routers.portainer-secure.rule=Host(`pihole.domain.be`)"
        - "traefik.http.routers.portainer-secure.tls=true"
        - "traefik.http.routers.portainer-secure.service=portainer"
        - "traefik.http.services.portainer.loadbalancer.server.port=9000"
        - "traefik.docker.network=proxy"
    ```