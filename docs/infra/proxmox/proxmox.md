# Proxmox Cheat-Sheet

Proxmox is een krachtige open-source serveroplossing die verschillende producten omvat.

- Proxmox VE is een platform voor enterprise virtualisatie dat KVM en Linux Containers (LXC) integreert.
- Proxmox Mail Gateway is een e-mailbeveiligingsoplossing die je mailserver beschermt tegen alle e-maildreigingen.
- Proxmox Backup Server is een backupoplossing voor het efficiënt back-uppen en herstellen van VM’s, containers en fysieke hosts.

Je kunt Proxmox software downloaden van hun [website](https://pve.proxmox.com).

![logo](https://upload.wikimedia.org/wikipedia/commons/f/f6/Proxmox-VE-4-4-screenshot-startpage.png)

## Installatie van Proxmox VE

Volg de stappen hieronder om Proxmox VE te installeren op een fysieke of dedicated server.

### Stap 1: Download het Proxmox ISO-bestand

De eerste stap is om het Proxmox VE ISO-bestand te downloaden.

- Ga naar de officiële Proxmox Downloads pagina en selecteer **Proxmox Virtual Environment**.
- Dit brengt je naar het Proxmox Virtual Environment Archive dat ISO-bestanden en officiële documentatie opslaat. Selecteer **ISO Images** om verder te gaan.
- Op het moment van schrijven is de nieuwste versie van de Proxmox VE ISO Installer 7.1. Als er een nieuwere versie beschikbaar is, staat deze bovenaan. Klik op **Download** en sla het bestand op.

### Stap 2: Bereid het installatiemedium voor

Kopieer het Proxmox ISO-bestand op een CD/DVD of een USB-stick.

- Hoewel beide opties mogelijk zijn, wordt aangenomen dat de meeste systemen geen optische drive hebben.
- Steek de USB-stick in en kopieer het ISO-bestand naar de USB-stick met behulp van de opdrachtregel of een USB-formatteringstool (zoals Etcher of Rufus).

!!! alert
    Let op: Zorg ervoor dat je USB-stick minstens 1 GB aan opslagruimte heeft. Maak ook een back-up van en verwijder alle gegevens op het apparaat, want het proces zal alle eerder opgeslagen gegevens wissen.

Als je op Linux werkt, is de snelste manier om een opstartbare USB te maken, de volgende opdracht uit te voeren:

```sh
dd bs=1M conv=fdatasync if=./proxmox-ve_*.iso of=/device/name
```

Pas indien nodig de bestandsnaam en het pad aan in `if=./proxmox-ve_*.iso` en zorg ervoor dat je de juiste apparaatnaam opgeeft in `of=/device/name`.

Om de naam van je USB-stick te vinden, voer je de volgende opdracht uit voor en na het inpluggen van het apparaat:

```sh
lsblk
```

Vergelijk de uitvoer. De extra vermelding in de tweede uitvoer is de naam van het apparaat.

### Stap 3: Start de Proxmox Installer

- Ga naar de server (machine) waarop je Proxmox wilt installeren en steek de USB-stick in.
- Terwijl de server opstart, ga je naar het opstartmenu door op de vereiste toets(en) te drukken. Meestal zijn dit Esc, F2, F10, F11 of F12.
- Selecteer het installatiemedium met het Proxmox ISO-bestand en start ervan op.
- Vervolgens verschijnt het Proxmox VE-menu. Selecteer **Install Proxmox VE** om de standaardinstallatie te starten.

### Stap 4: Voer Proxmox uit

- Volg de instructies op het scherm om Proxmox te installeren. Je moet onder andere akkoord gaan met de licentieovereenkomst, een doelschijf selecteren, een land en tijdzone kiezen, een wachtwoord instellen en een netwerkconfiguratie invoeren.
- Na het voltooien van de installatie wordt je gevraagd om de server opnieuw op te starten. Verwijder het installatiemedium en klik op **Reboot**.
- Zodra de server is herstart, zie je een scherm met informatie over hoe je toegang kunt krijgen tot Proxmox via een webconsole of een commandoregel.

### Stap 5: Maak een VM

- Om toegang te krijgen tot Proxmox via een webconsole, open je een browser en ga je naar het adres dat wordt weergegeven op het scherm, bijvoorbeeld `https://192.168.1.100:8006`. Je moet een beveiligingswaarschuwing accepteren om door te gaan.
- Log in met de gebruikersnaam root en het wachtwoord dat je tijdens de installatie hebt ingesteld.
- Je ziet nu het Proxmox-dashboard, waar je virtuele machines en containers kunt maken en beheren.
- Om een nieuwe VM te maken, klik je op **Create VM** in de rechterbovenhoek. Volg de wizard om de VM te configureren met de gewenste opties, zoals naam, besturingssysteem, schijf, netwerk, enz.
- Nadat je de VM hebt gemaakt, kun je deze starten, stoppen, klonen, verwijderen of wijzigen met behulp van de knoppen in het paneel.

## Configuratie van Proxmox VE

Hier zijn enkele veelvoorkomende taken die je kunt uitvoeren om Proxmox VE naar wens te configureren.

### Start de VM bij het opstarten

- Als je wilt dat een VM automatisch wordt gestart wanneer de host wordt opgestart, kun je dit inschakelen in de instellingen van de VM.
- Selecteer de VM in het linkerpaneel en klik op **Options**. Klik vervolgens op **Start at boot** en selecteer **Yes** in het vervolgkeuzemenu.

### Vergroot/verklein de virtuele schijfgrootte

- Als je de grootte van de virtuele schijf van een VM wilt wijzigen, moet je eerst de VM uitschakelen.
- Selecteer de VM in het linkerpaneel en klik op **Hardware**. Klik vervolgens op **Hard Disk** en klik op **Resize disk** in het menu.
- Voer de nieuwe grootte in die je wilt toewijzen aan de schijf en klik op **Resize**.

!!! note
    Let op: je kunt de schijf alleen vergroten, niet verkleinen.

### Schakel NAT-netwerkmodus in

- Als je wilt dat je VM’s verbinding kunnen maken met internet via een gedeeld IP-adres van de host, kun je NAT-netwerkmodus inschakelen.
- Selecteer de host in het linkerpaneel en klik op **System**. Klik vervolgens op **Network** en klik op **Create** in het menu.
- Selecteer **Linux Bridge** als type en voer een naam in voor de brug, bijvoorbeeld `vmbr1`. Schakel het selectievakje **NAT** in en klik op **Create**.
- Selecteer nu een VM die je wilt verbinden met internet via NAT en klik op **Hardware**. Klik vervolgens op **Network Device** en klik op **Edit** in het menu.
- Selecteer `vmbr1` als brug en klik op **OK**. Herhaal dit voor elke VM die je wilt verbinden met internet via NAT.

## Links

- [Proxmox VE Documentation](https://pve.proxmox.com/pve-docs/)
- [Install Proxmox VE {Step-by-Step Guide} - phoenixNAP](https://phoenixnap.com/kb/install-proxmox)
- [Installation - Proxmox VE](https://pve.proxmox.com/wiki/Installation)

## VM Beheer

### Basic

```shell
# Lijst van vm's
qm list

# Maak of restore een vm.
qm create <vmid>

# start een VM
qm start <vmid>

# vm opschorten.
qm suspend <vmid>

# vm uitschakelen
qm shutdown <vmid>

# vm herstarten
qm reboot <vmid>

# vm resetten
qm reset <vmid>

# vm stoppen
qm stop <vmid>

# Vernietig de VM en alle gebruikte/eigen volumes.
# Verwijdert alle vm-specifieke machtigingen en firewallregels
qm destroy <vmid>

# Ga naar de Qemu Monitor interface.
qm monitor <vmid>

# Haal de configuratie van de vm op met zowel de huidige als de in behandeling zijnde waarden.
qm pending <vmid>

# Stuur key event naar vm.
qm sendkey <vmid> <key> [OPTIONS]

# Laat de command line zien die wordt gebruikt om de VM te starten (debug info).
qm showcmd <vmid> [OPTIONS]

# Ontgrendel de vm.
qm unlock <vmid>

# Clone een VM
qm clone <vmid> <newid>

# Migreer een VM
qm migrate <vmid> <target-node>

# Toon VM status
qm status <vmid>

# Resources voor een vm opschonen
qm cleanup <vmid> <clean-shutdown> <guest-requested>

# Maak een template
qm template <vmid> [OPTIONS]

# Opties voor virtuele machines instellen (synchrone API)
qm set <vmid> [OPTIONS]
```

### Cloudinit

```shell
# Get automatically generated cloudinit config.
qm cloudinit dump <vmid> <type>

# Get the cloudinit configuration with both current and pending values.
qm cloudinit pending <vmid>

# Regenerate and change cloudinit config drive.
qm cloudinit update <vmid>
```

### Disk

```shell
# Importeer een externe schijfkopie als een ongebruikte schijf in een VM.
# Het afbeeldingsformaat moet worden ondersteund door qemu-img(1).
qm disk import <vmid> <source> <storage>

# Verplaats het volume naar een andere opslag of naar een andere VM.
qm disk move <vmid> <disk> [<storage>] [OPTIONS]

# Scan alle opslag opnieuw en update schijfgroottes en ongebruikte schijfkopieën.
qm disk rescan [OPTIONS]

# Vergroot de volume.
qm disk resize <vmid> <disk> <size> [OPTIONS]

# Unlink/verwijder disk images.
qm disk unlink <vmid> --idlist <string> [OPTIONS]

# rescan volumes
qm rescan
```

## Web GUI

```shell
# Herstart web GUI
service pveproxy restart
```

## Resize Disk

### Vergroot de schijfgrootte

Vergroot de schijfgrootte in de GUI of met de volgend command:

```shell
qm resize 100 virtio0 +5G
```

### Verklein Schijfgrootte

> Voordat u de schijfgrootte in Proxmox verkleint, moet u een back-up maken!

1. Converteer qcow2 naar raw

  ```shell
  qemu-img convert vm-100.qcow2 vm-100.raw
  ```

2. Verklein de disk

```shell
qemu-img resize -f raw vm-100.raw 10G
```

3. Converteer terug naar qcow2

```shell
qemu-img convert -p -O qcow2 vm-100.raw vm-100.qcow2
```

## Disk Management

### Ubuntu VM Disk vergroten in Proxmox

Als je in Proxmox een disk vergroot, zal deze door de OS nog niet volledig gebruikt kunnen worden.
Daarom moeten we, na de vergroting in proxmox, deze alsnog met [GParted](../tools/gparted.md) en via de CLI resizen zodat de volledige grootte herkend kan worden door het OS.

#### 1. Virtuele machine uitzetten

Schakel de virtuele machine eerst helemaal uit.

#### 2. Harde schijf resizen

Nu kan je de harde schijf vergroten.

`Proxmox` > `Hardware Settings`

#### 3. GParted downloaden en opstarten

1. Download laatste Gparted ISO van de [GParted download website](https://gparted.org/download.php)
2. Importeer de GParted ISO file in Proxmox
3. Start de VM en laat deze starten vanaf de ISO. (Duw snel op ++esc++)

#### 4. Partitie resizen met GParted

1. Zoek de correcte partitie
2. Resize de partitie (++right-button++ -> resize)
3. Duw opt vingske 🙂

#### 5. CLI resizing

Check de huidige disksize met commando:

```bash
df -h
```

Laat Ubuntu de volledige disk gebruiken:

```bash
sudo /sbin/lvresize -l +100%FREE /dev/mapper/ubuntu--vg-ubuntu--lv
```

Gebruik resize2fs op deze disk

```bash
sudo /sbin/resize2fs /dev/mapper/ubuntu--vg-ubuntu--lv
```

Hierna kan je nogmaals de disksize bekijken.

```bash
df -h
```

Als er geen fouten opgetreden zijn, heb je nu net de harde schijf vergroot!

### Snapshot

```shell
# List all snapshots.
qm listsnapshot <vmid>

# Snapshot a VM
qm snapshot <vmid> <snapname>

# Delete a snapshot.
qm delsnapshot <vmid> <snapname>

# Rollback a snapshot
qm rollback <vmid> <snapname>

# Open a terminal using a serial device
# (The VM need to have a serial device configured, for example serial0: socket)
qm terminal <vmid> [OPTIONS]

# Proxy VM VNC traffic to stdin/stdout
qm vncproxy <vmid>
```

### Misc

```shell
# Execute Qemu Guest Agent commands.
qm guest cmd <vmid> <command>

# Executes the given command via the guest agent
qm guest exec <vmid> [<extra-args>] [OPTIONS]

# Gets the status of the given pid started by the guest-agent
qm guest exec-status <vmid> <pid>

# Sets the password for the given user to the given password
qm guest passwd <vmid> <username> [OPTIONS]
```

### PV, VG, LV Management

```shell
# Create a PV
pvcreate <disk-device-name>

# Remove a PV
pvremove <disk-device-name>

# List all PVs
pvs

# Create a VG
vgcreate <vg-name> <disk-device-name>

# Remove a VG
vgremove <vg-name>

# List all VGs
vgs

# Create a LV
lvcreate -L <lv-size> -n <lv-name> <vg-name>

# Remove a LV
lvremove <vg-name>/<lv-name>

# List all LVs
lvs
```

### Storage Management

```shell
# Create a new storage.
pvesm add <type> <storage> [OPTIONS]

# Allocate disk images.
pvesm alloc <storage> <vmid> <filename> <size> [OPTIONS]

# Delete volume
pvesm free <volume> [OPTIONS]

# Delete storage configuration.
pvesm remove <storage>

# List storage content.
pvesm list <storage> [OPTIONS]

# An alias for pvesm scan lvm.
pvesm lvmscan

# An alias for pvesm scan lvmthin.
pvesm lvmthinscan

# List local LVM volume groups.
pvesm scan lvm

# List local LVM Thin Pools.
pvesm scan lvmthin <vg>

# Get status for all datastores.
pvesm status [OPTIONS]
```

### Template Management

```shell
# list all templates
pveam available

# list all templates
pveam list <storage>

# Download appliance templates
pveam download <storage> <template>

# Remove a template.
pveam remove <template-path>

# Update Container Template Database.
pveam update
```

## Certificate Management

See the [Proxmox Certificate Management](proxmox-certificate-management.md) cheat sheet.

## Container Management

### Basic

```shell
# List containers
pct list

# Create or restore a container.
pct create <vmid> <ostemplate> [OPTIONS]

# Start the container.
pct start <vmid> [OPTIONS]

# Create a container clone/copy
pct clone <vmid> <newid> [OPTIONS]

# Suspend the container. This is experimental.
pct suspend <vmid>

# Resume the container.
pct resume <vmid>

# Stop the container.
# This will abruptly stop all processes running in the container.
pct stop <vmid> [OPTIONS]

# Shutdown the container.
# This will trigger a clean shutdown of the container, see lxc-stop(1) for details.
pct shutdown <vmid> [OPTIONS]

# Destroy the container (also delete all uses files).
pct destroy <vmid> [OPTIONS]

# Show CT status.
pct status <vmid> [OPTIONS]

# Migrate the container to another node. Creates a new migration task.
pct migrate <vmid> <target> [OPTIONS]

# Get container configuration.
pct config <vmid> [OPTIONS]

# Print the list of assigned CPU sets.
pct cpusets

# Get container configuration, including pending changes.
pct pending <vmid>

# Reboot the container by shutting it down, and starting it again. Applies pending changes.
pct reboot <vmid> [OPTIONS]

# Create or restore a container.
pct restore <vmid> <ostemplate> [OPTIONS]

# Set container options.
pct set <vmid> [OPTIONS]

# Create a Template.
pct template <vmid>

# Unlock the VM.
pct unlock <vmid>
```

### Container Disks

```shell
# Get the container?s current disk usage.
pct df <vmid>

# Run a filesystem check (fsck) on a container volume.
pct fsck <vmid> [OPTIONS]

# Run fstrim on a chosen CT and its mountpoints.
pct fstrim <vmid> [OPTIONS]

# Mount the container?s filesystem on the host.
# This will hold a lock on the container and is meant for emergency maintenance only
# as it will prevent further operations on the container other than start and stop.
pct mount <vmid>

# Move a rootfs-/mp-volume to a different storage or to a different container.
pct move-volume <vmid> <volume> [<storage>] [<target-vmid>] [<target-volume>] [OPTIONS]

# Unmount the container?s filesystem.
pct unmount <vmid>

# Resize a container mount point.
pct resize <vmid> <disk> <size> [OPTIONS]

# Rescan all storages and update disk sizes and unused disk images.
pct rescan [OPTIONS]

# Connect to container
pct enter <vmid>

# Launch a console for the specified container.
pct console <vmid> [OPTIONS]

# Launch a shell for the specified container.
pct enter <vmid>

# Launch a command inside the specified container.
pct exec <vmid> [<extra-args>]

# Copy a file from the container to the local system.
pct pull <vmid> <path> <destination> [OPTIONS]

# Copy a local file to the container.
pct push <vmid> <file> <destination> [OPTIONS]
```

### Container Snapshot

```shell
# Snapshot a container.
pct snapshot <vmid> <snapname> [OPTIONS]

# List all snapshots.
pct listsnapshot <vmid>

# Rollback LXC state to specified snapshot.
pct rollback <vmid> <snapname> [OPTIONS]

# Delete a LXC snapshot.
pct delsnapshot <vmid> <snapname> [OPTIONS]
```

## Web GUI

```shell
# Restart web GUI
service pveproxy restart
```

## Resize Disk

### Increase disk size

Increase disk size in the GUI or with the following command

```shell
qm resize 100 virtio0 +5G
```

### Decrease disk size
>
> Before decreasing disk sizes in Proxmox, you should take a backup!

1. Convert qcow2 to raw

```shell
qemu-img convert vm-100.qcow2 vm-100.raw
```

2. Shrink the disk

```shell
qemu-img resize -f raw vm-100.raw 10G
```

3. Convert back to qcow2#

```shell
qemu-img convert -p -O qcow2 vm-100.raw vm-100.qcow2
```

## Further information

More examples and tutorials regarding Proxmox can be found in the link list below:

- Ansible playbook that automates Linux VM updates running on Proxmox (including snapshots): [TheDatabaseMe - update_proxmox_vm](https://github.com/thedatabaseme/update_proxmox_vm)
- Manage Proxmox VM templates with Packer: [Use Packer to build Proxmox images](https://thedatabaseme.de/2022/10/16/what-a-golden-boy-use-packer-to-build-proxmox-images/)

## Plex on Proxmox

Bekijk deze [note](../misc/plex_proxmox.md) voor meer informatie om Plex via LXC te installeren op Proxmox

## Post-installatie script

[script](https://github.com/extremeshok/xshok-proxmox)

## Proxmox VE Helper Scripts

Bekijk [deze website](https://tteck.github.io/Proxmox/) voor enkele interessante scripts.
