# Linux snippets

## Beschrijving

Dit bestand bevat een verzameling van handige Ubuntu snippets die ik heb gevonden of gemaakt. Ze kunnen worden gebruikt om verschillende taken uit te voeren op ubuntu systemen, zoals het installeren van pakketten, het configureren van netwerken, het beheren van processen en nog veel meer

## Laat de 5 laatst bewerkte bestanden zien

```bash
ls -lht | head -6
```

### Uitleg

`-l` geeft de uitvoer in een lijstformaat weer

`-h` maakt de uitvoer leesbaar voor mensen (d.w.z. bestandsgroottes verschijnen in kb, mb, etc.)

`-t` sorteert de uitvoer door het meest recent gewijzigde bestand eerst te plaatsen

`head -6` zal 5 bestanden laten zien omdat ls de blokgrootte in de eerste regel van de uitvoer afdrukt.

!!! note "Voorbeeld resultaat"
    total 26960312 -rw-r--r--@ 1 user staff 1.2K 11 Jan 11:22 phone2.7.py -rw-r--r--@ 1 user staff 2.7M 10 Jan 15:26 03-cookies-1.pdf -rw-r--r--@ 1 user staff 9.2M 9 Jan 16:21 Wk1_sem.pdf -rw-r--r--@ 1 user staff 502K 8 Jan 10:20 lab-01.pdf -rw-rw-rw-@ 1 user staff 2.0M 5 Jan 22:06 0410-1.wmv

## Set static IP

```bash
sudo nano /etc/netplan/00-installer-config.yaml
```

```yaml
# This file describes the network interfaces available on your system
# For more information, see netplan(5).
network:
  version: 2
  renderer: networkd
  ethernets:
    ens33:                #pas dit aan
      dhcp4: no                #yes/no
      dhcp6: no                #yes/no
      addresses: [192.168.1.100/24]    #pas dit aan
      gateway4: 192.168.1.1        #pas dit aan
      nameservers:
        addresses: [8.8.8.8,8.8.4.4]
```

```bash
sudo netplan apply
```

## Upgrade Ubuntu 20.04 to 22.04

```bash
sudo apt update & sudo apt upgrade -y
sudo do-release-upgrade
```

## Sluit een process op een gegeven poort

```bash
sudo kill -9 `sudo lsof -t -i:9001`
```

## Log all Bash commando's in een file

Om alle bash commando's bij te houden in een file geef je volgend commando in:

```bash
trap 'echo "$USER":"$BASH_COMMAND" >>/path/to/log' DEBUG
```
