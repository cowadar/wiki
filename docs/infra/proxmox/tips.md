# Voordat ik iets op Proxmox doe, doe ik eerst...

## Updates

Bewerk de file `/etc/apt/sources.list`

### Proxmox-versie 6.X

```sh
deb http://ftp.us.debian.org/debian buster main contrib

deb http://ftp.us.debian.org/debian buster-updates main contrib

# security updates
deb http://security.debian.org buster/updates main contrib

# not for production use
deb http://download.proxmox.com/debian buster pve-no-subscription
```

### Proxmox-versie 7.X

```sh
deb http://ftp.debian.org/debian bullseye main contrib

deb http://ftp.debian.org/debian bullseye-updates main contrib

# security updates
deb http://security.debian.org/debian-security bullseye-security main contrib

# PVE pve-no-subscription repository provided by proxmox.com,
# NOT recommended for production use
deb http://download.proxmox.com/debian/pve bullseye pve-no-subscription
```

Bewerk `/etc/apt/sources.list.d/pve-enterprise.list`

```sh
# deb https://enterprise.proxmox.com/debian/pve buster pve-enterprise
```

Voer volgende commando's uit:

```sh
apt-get update
apt dist-upgrade
reboot
```

## Storage

!!! danger
WEES VOORZICHTIG. Hiermee worden uw schijven gewist.

```sh
fdisk /dev/sda
```

Dan `P` voor **partitie**, dan `D` voor **verwijderen**, dan `W` voor **schrijven**.

Controleer SMART Monitoring

```sh
smartctl -a /dev/sda
```

## VLAN bewust
Als u uw VLANS wilt beperken

```sh
nano /etc/network/interfaces
```

Stel hier uw VLAN in
```sh
bridge-vlan-aware yes
bridge-vids 20
```

## Voorbeeld van het NIC-team
```sh
nano /etc/network/interfaces
```

```config
auto eno1
iface eno1 inet manual

auto eno2
iface eno2 inet manual

auto bond0
iface bond0 inet manual
bond-slaves eno1 eno2
bond-miimon 100
bond-mode 802.3ad
bond-xmit-hash-policy layer2+3

auto vmbr0
iface vmbr0 inet static
address 192.168.0.11/24
gateway 192.168.0.1
bridge-ports bond0
bridge-stp off
bridge-fd 0
bridge-vlan-aware yes
bridge-vids 2-4094
#lacp nic team
```

Als u Proxmox 7 gebruikt, bekijk dan hier de [gewijzigde configuratie](https://techno-tim.github.io/posts/proxmox-7/) voor LAGG / LACP