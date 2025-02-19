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


## Ful Docker compose (easy)

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
  └── docker-compose
      └──docker-compose.yaml
```

Ze het volgend in de "docker-compose.yaml"
```yaml 
services:
  mariadb:
    image: lscr.io/linuxserver/mariadb:latest
    container_name: mariadb
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