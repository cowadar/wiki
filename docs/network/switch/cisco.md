---
title: Cisco 
---

## Cisco

Een Cisco switch is een netwerkapparaat dat wordt geproduceerd door het bedrijf Cisco. Een switch wordt gebruikt om meerdere apparaten in een netwerk met elkaar te verbinden en gegevens tussen deze apparaten te verzenden. Cisco produceert verschillende soorten switches, waaronder unmanaged switches, smart switches en managed switches, die elk verschillende functies en mogelijkheden bieden.

### TP-link AP config

Door een access point gaan verschillende VLANs. waardoor je ook in de config deze moet megeven.
Je hebt 2 manieren ofwel definjeer je welke VLANs `switchport trunk allow vlan x,x` of je laat dit weg en dan laat hij alle vlans door. De native vlan is de vlan waar de Accepoint zijn ip vandaan haalt.

```bash
switchport trunk native vlan 10
switchport mode trunk
spanning-tree portfast
```
