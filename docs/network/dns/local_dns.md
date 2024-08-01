# Local DNS

## Wat is local DNS?

Local DNS is een systeem dat een domeinnaam zoals example.com vertaalt naar het numerieke IP-adres van de server waar de inhoud zich bevindt. Wanneer je een netwerkverzoek doet met een domeinnaam, zal je systeem een DNS-zoekopdracht uitvoeren om het serveradres te bepalen waarmee het contact moet opnemen. Dit voegt een extra vertraging toe aan elk verzoek dat je doet.

Je kunt je eigen DNS-server draaien om meer controle te hebben over je netwerk. Een veelvoorkomende reden is dat je netwerk-niveau domeinmappings kunt configureren, zoals web-server naar `192.168.0.101`. Door je router zo in te stellen dat hij je DNS gebruikt, kun je met elk van je verbonden apparaten toegang krijgen tot `192.168.0.101` via `http://web-server.example.com`

## Hoe Local DNS opzetten

Dit is heel simpel op te zetten door middel van volgende tools:

- [PiHole](pihole.md)
- [PfSense](../router/pfsense.md)
- [Unifi](../router/unifi.md)
- [Windows server](../../windows/windows_server.md)
- andere dns software

### Praktische voorbeelden

`tower.lan` -> `172.16.0.5`

`router1.lan` -> `192.168.1.11`

## PiHole

Hier heb ik een voorbeeld van hoe je dit configureerd in PiHole.
![Pihole_local_dns](../../
## Suffix

### Wat is een DNS Suffix?

Een DNS-Suffix is het eerste deel van een domeinnaam, zoals `example.com`. Het wordt gebruikt om een domein te identificeren en te onderscheiden van andere domeinen.

Geef in een command prompt volgende in:

```bash
ipconfig /all
```

![Voorbeeld cmd](../../_assets/images/localdns_cmd.png)
> Je zult zien dat bij iedere netwerkadapter (wifi, eth, vpn,...) er `Connection-specific DNS Suffix` zal staan.

In dit voorbeeld staat hier `lan`. Dat wil zeggen dat ik dus met mijn browser kan navigeren naar `tower\`, die uitkomt op mijn server, genaamd `tower`.
Je moet je geen zorgen maken over de poorten, want elke service zal automatisch volgende poorten aanspreken:

- RDP: `3389`
- ssh: `22`
- http: `80`
- minecraft: `25565`

!!! note
    Local DNS houd geen rekening met custom poorten voor een service.
    Dit kan je wel op andere manieren oplossen.(comming soon).

Je kan meer dan 1 suffix statisch instellen of meegeven met je DHCP release.
Als je dit in [Wireshark](../tools/wireshark.md) zou capturen zal je zien dat hij achter `tower` alle suffixen gaat plakken en dat over het netwerk naar de local dns server zal sturen. Dit kan je capturen volgens [deze](../tools/wireshark.md#local-dns-prefix-capturen) methode:

![foto van wireshark capture](../../_assets/images/local_dns_tower_suffix.png)

![foto van wireshark capture](../../_assets/images/wireshark_tower_suffix.png)



## Local DNS met custom poorten
