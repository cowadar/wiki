# HashiCorp Vault

## Beschrijving

HashiCorp Vault is een populaire open-source tool die is ontworpen om gevoelige gegevens veilig op te slaan en te beheren, zoals wachtwoorden, API-sleutels en encryptiesleutels. Het biedt een gecentraliseerd systeem voor het beheren van geheimen en biedt fijnmazige toegangscontrole om gevoelige informatie te beschermen.

Vault biedt verschillende authenticatiemethoden, waaronder tokens, gebruikersnaam/wachtwoord en vertrouwde entiteiten (bijv. GitHub, Active Directory). Het ondersteunt ook 'encryption-as-a-service', dynamische geheimen en het leasen van geheimen om de beveiliging te verbeteren en geheimen efficiënt te beheren.

## Kenmerken

- **Geheimenbeheer:** Vault slaat gevoelige gegevens veilig op en beheert ze, waarbij geheimen worden versleuteld tijdens opslag.
- **Dynamische geheimen:** Vault kan dynamische geheimen genereren voor verschillende services, waardoor het risico van langdurige geheimen wordt verminderd.
- **Encryption-as-a-Service:** Vault fungeert als een centraal sleutelbeheersysteem en biedt versleutelingsservices voor toepassingen.
- **Leasen en verlengen:** Geheimen kunnen worden geleaset voor een bepaalde duur, en Vault biedt automatische verlengingsmogelijkheden.
- **Toegangsbeheerbeleid:** Vault biedt fijnmazige toegangsbeheerbeleidsregels om gebruikerstoegang tot geheimen te beperken en te beheren.
- **Audit logging:** Alle interacties met Vault worden gelogd, waardoor een audittrail wordt gecreëerd voor compliance-doeleinden.

## Installatie

### Vereisten

Voordat je Vault installeert, zorg ervoor dat je aan de volgende vereisten voldoet:

- Een ondersteund besturingssysteem (bijv. Linux, macOS, Windows)
- Beheerdersrechten op het systeem waarop Vault wordt geïnstalleerd

### Installatiestappen

1. **Download Vault:** Ga naar de [HashiCorp Vault-website](https://www.vaultproject.io/) en download de nieuwste versie van Vault die geschikt is voor jouw besturingssysteem.

2. **Pak het archief uit:** Pak het gedownloade archiefbestand uit naar een map van jouw keuze.

   ```bash
   $ tar -xf vault_<versie>.zip
   ```

3. **Verplaats het uitgepakte bestand:** Navigeer naar de uitgepakte Vault-directory.

   ```bash
   $ cd vault_<versie>
   ```

4. **Plaats het uitvoerbare bestand:** Verplaats het uitvoerbare bestand naar een locatie in je `PATH`, zodat je Vault vanaf elke locatie kunt uitvoeren.

   ```bash
   $ sudo mv vault /usr/local/bin/
   ```

5. **Verifieer de installatie:** Controleer of Vault correct is geïnstalleerd door het uitvoeren van het volgende commando.

   ```bash
   $ vault -v
   ```

   Je zou de versie van Vault moeten zien die je hebt geïnstalleerd.

6. **Start de Vault-server:** Om de Vault-server te starten, voer je het volgende commando uit.

   ```bash
   $ vault server -dev
   ```

   Dit start een ontwikkelingsserver van Vault in de dev-modus. Deze modus is bedoeld voor lokaal ontwikkelings- en testgebruik.

7. **Gebruik Vault:** Je kunt nu de Vault CLI gebruiken om met de server te communiceren en geheimen te beheren. Raadpleeg de [Vault-documentatie](https://www.vaultproject.io/docs) voor meer informatie over het gebruik van Vault en het configureren van geheimen.

   ```bash
   $ vault <commando>
   ```

   Bijvoorbeeld: `$ vault secrets list` om de lijst met geheime backends weer te geven.

   Let op: De ontwikkelingsserver van Vault in de dev-modus is niet geschikt voor productiegebruik. Voor productie-implementaties raadpleeg je de Vault-documentatie voor informatie over het instellen van een productieklare Vault-server.

## links

- [https://www.youtube.com/watch?v=WQ52YJMZZYY](https://www.youtube.com/watch?v=WQ52YJMZZYY)