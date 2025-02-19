# Docker Swarm

## Wat is Docker Swarm
Docker Swarm is een clustering- en orkestratietool voor Docker-containers. Het maakt het mogelijk om meerdere Docker-hosts te groeperen en deze als één logische eenheid te beheren.

Belangrijkste kenmerken van Docker Swarm:

- Container Orchestratie: Automatiseert de uitrol, schaling en het beheer van containers.
- Load Balancing: Verdeelt verkeer automatisch over de beschikbare containers.
- High Availability: Containers kunnen worden gerepliceerd over meerdere nodes, zodat de applicatie blijft draaien bij uitval.
- Rolling Updates: Voert updates uit zonder downtime.

Swarm is ingebouwd in Docker en eenvoudiger dan Kubernetes, maar biedt minder geavanceerde functies. Het is vooral handig voor kleinere tot middelgrote projecten waar eenvoud en snelle setup belangrijk zijn.
### Docker Swarm opzetten
In Docker Swarm zijn er twee soorten nodes:  

#### 1. Manager Node 🏗️  
- Beheert het cluster en verdeelt taken (orkestratie).  
- Kan services aanmaken, schalen en beheren.  
- Gebruikt Raft-consensus voor hoge beschikbaarheid.  

#### 2. Worker Node 🏭  
- Voert de toegewezen taken (containers) uit.  
- Kan geen beslissingen nemen over orkestratie.  
- Meldt status terug aan de manager.  

**Kort gezegd:** **Managers beheren, workers voeren uit.** 🚀

Om een Docker swarm op te zetten heb je 2 commando`s nodig.
Dit voor je uit op de Manager.
```bash
docker swarm init
```
Nu krijg je een join code en deze plak je op de Nodes.
Als je deze code vergeten bent kan je volgende commando uitvoeren op de manager.
```bash
docker swarm join-token worker
```
Wil ke een extra manager wilt toevoegen kan je volgende commando op de huidige manage uitvoeren.
```bash
docker swarm join-token manager
```


## Swarm Networks

### Wat zijn Swarm networks
Docker Swarm-netwerken bepalen hoe containers binnen een Swarm-cluster met elkaar en met de buitenwereld communiceren. Er zijn drie hoofdtypen netwerken:  

#### 1. Overlay Network 🔄  
- Verbindt containers over meerdere nodes.  
- Standaard voor services in Swarm Mode.  
- Zorgt voor interne service-discovery en load balancing.  

#### 2. Ingress Network 🌍  
- Specifiek voor het routeren van inkomend verkeer naar de juiste service.  
- Gebruikt automatisch een ingebouwde load balancer.  

#### 3. Bridge Network (per node) 🔗  
- Alleen binnen één node bruikbaar.  
- Containers kunnen communiceren, maar niet over meerdere nodes heen.  

Swarm-netwerken maken het makkelijk om containers dynamisch te laten samenwerken zonder dat je handmatig IP-adressen hoeft te beheren.

!!! note
    Je kan je huidige docker containers het swarm netwerk laten gebruiken.
    Als je een Swarm network aanmaakt zien uw nodes deze pas als er een conatainer op de node deze gebruikt.

### Aanmaken van een Docker Swarm overlay network
Door een Docker Swarm network aan te maken dat ook attacheble is aan uw huide docker conainers gebruik je het volgende commando op de manager.
```bash
docker network create -d overlay --attachable my_overlay_network
```

Het enigste dat je nu moet doen is dit netwerk in je compose steken van de stand alone docker containers die je met elkaar wilt laten praten.

```yaml
services:
    app:
        networks:
            - swarm-network

networks:
  swarm-network:
    external: true
```

### Testen Docker Swarm network
Om een Swarm netwerk te testen over verschillende nodes moet je het ping commando gebruiken en de conatiner naan of (docker network)IP gebruiken.
Dat doe je met volgende commando.

```bash
docker exec -it webserver ping <dns/ip>
```


## Docker swarm containers
### Comming soon