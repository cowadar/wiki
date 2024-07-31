# Site to Site (S2S)

Een Site to Site maakt het heel gemakkelijk zoals in een bedrijf om 2 locaties veilig met elkaar te verbinden alsof het 1 locatie (en dus netwerk) is.
Daarom hebben wij er voor gekozen om onze 2 locaties met elkaar te verbinden.
Zo kunnen we onze Docker services gemakkelijk delen zonder **over** het internet te gaan.

Onze huidige configuratie is opgemaakt met [Pfsense](../router/pfsense.md) en [Unifi](../router/unifi.md).

Laten we eerst even oplijsten wat onze vereisten zijn om een S2S op te zetten.

!!! note
    Zie wel dat je een bridge modem heb zodat jouw firewall een **WAN** IP heeft zodat jij de modem bent.

- Phase1
  - Version: IKEv1/2
  - Remote IP: WAN IP van de andere locatie
  - Authenticatie: PSK/Certificaat
  - Encryption: AES/3DES/...
  - Life time: 28800
  - Dead peer Detection: Optioneel kijk of de andere kant nog op is.
- Phase2
  - Local network: host/range/network
  - Remote netwerk: Lokale Subnet aan de remote kant (Mag niet gelijk zijn aan dat van jou)
  - Protocol: ESP/AH
  - Encryption: AES/3DES/...
  - Hash: MD5/sha...
  - PFS: Alles hoger dan 14
  - Life time: 3600

## Instellen

### PfSense

!!! note
    Zie wel dat je bij de encryptie ook de `AESxxx-GCM` opzet. Je hebt geen keus om dit bij Unifi in te stellen.

1. VPN intellingen
    - VPN --> IPSec --> Add P1 (Phase 1)
    - VPN --> IPSec --> Add P2 (Phase 2)

2. Kijk vervolgend in de logs of de VPN online is gekomen.
    - Status --> System Logs --> IPSec

3. Als de VPN onlineis  ga je nog rules moeten opzetten.
    - Firewall --> Rules --> IPSec
        - hier stel je de routing net als dat je dat doet bij andere VLANs
!!! note
    Hou rekening met je andere rules als je in een vlan/subnet aanduid dat die niet aan alles kan kan die daar ook geblokeerd worden.
    Als het dan nog niet werkt kan je terug in de logs gaan kijken maar bij Firewall --> Normal view.
    Hier kan je dan op `+` klikken en gaat die voor dat IP zelf een rule aanmaken.
    Vervolgend kan je terug naar rules gaan. Deze rule zoeken en aanspassen naar het hele subnet b.v.

### Unifi

1. Eerst gaan we de VPN instellen.
    - Settings --> Teleport & VPN --> Site-to-Site-VPN --> Create Site-to-Site VPN
        - Enable Route-Based VPN
2. nu gaan we de routing instellen

    - Setting --> Firewall & Security --> LAN --> Create New Rule

!!! note
    Er vanuitgaand dat jouw Firewall correct is ingesteld.