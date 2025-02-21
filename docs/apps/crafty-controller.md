# Crafty
## Wat is Crafty

**Crafty Controller** is een webgebaseerde beheerinterface voor Minecraft-servers. Het biedt een eenvoudige manier om meerdere Minecraft-servers te beheren, inclusief start/stop-functionaliteit, automatische updates en monitoring.

## 🌟 Belangrijkste kenmerken:
- ✅ **Webinterface** – Beheer servers via een browser  
- ✅ **Meerdere servers** – Ondersteuning voor verschillende Minecraft-instanties  
- ✅ **Automatische updates** – Houdt serverbestanden up-to-date  
- ✅ **Logboek en monitoring** – Houdt prestaties en fouten bij  
- ✅ **Gebruikersbeheer** – Toegangscontrole voor meerdere beheerders  

Handig voor zowel particuliere als professionele serverbeheerders! 🎮🚀

!!! note
    Dit kan je vergelijken met Prerodactyl maar dan enkel voor Minecraft server en een veel simpelere setup.

## Intallatie

### LXC
Wij hebben dit geinstalleerd in Proxmox met [Dit](https://community-scripts.github.io/ProxmoxVE/scripts?id=crafty-controller) script

Na het runnen van dit script kan je direkt beginnen met servers aanmaken.

### Docker
!!! warning
    Het nadeel van een Docker container is dat je de poorten zelf moet toevoegen aan het compose bestand.
    Dus zorg ervoor dat er genoeg poorten openstaan.

```yaml
services:
  crafty:
    container_name: crafty_container
    image: registry.gitlab.com/crafty-controller/crafty-4:latest
    restart: always
    environment:
        - TZ=Etc/UTC
    ports:
        - "8443:8443" # HTTPS
        - "8123:8123" # DYNMAP
        - "19132:19132/udp" # BEDROCK
        - "25500-25600:25500-25600" # MC SERV PORT RANGE
    volumes:
        - ./docker/backups:/crafty/backups
        - ./docker/logs:/crafty/logs
        - ./docker/servers:/crafty/servers
        - ./docker/config:/crafty/app/config
        - ./docker/import:/crafty/import
```
Voer het compose bestand uit
```bash
docker-compose up -d && docker-compose logs -f
```
# Update

```bash
docker-compose pull && docker-compose up -d
```