# GPU Passtrough

## Stap 1

Pas je grub aan als volgt:

### Cowarol setup

```bash
sudo nano /etc/default/grub
GRUB_CMDLINE_LINUX_DEFAULT="intel_iommu=on loglevel=3 vfio-pci.ids=01:00.1,01:00.0 quiet"
sudo grub-mkconfig -o /boot/grub/grub.cfg
```

### Bedar setup

```bash
sudo nano /etc/default/grub
GRUB_CMDLINE_LINUX_DEFAULT="intel_iommu=on quiet"
sudo grub-mkconfig -o /boot/grub/grub.cfg
```

!!! note
Bij Bedar werkte het de eerste optie niet. Daarom hebben we beide opties gegeven.

Als je de ID`s van je GPU PCI slot wilt weten, gebruik je volgend commando:

```bash
lspci
```

## Stap 2

Maak een nieuwe virtuele machine. Tijdens de wizard moet je aangeven dat je de `VM eerst nog wilt bewerken`.

### TAB: Overview

BIOS --> ../x64/OVMF_code.secboot.fd

### TAB: CPUs

- Current allocation <2
- Socket 1
- cores <2
- treads <1

### KNOP: Add Hardware

- TPM
- Model: CRB
- Backend: Emulated device
- Version 2.0

### Start VM

Nu zie je dat er problemen optreden.

#### Installeer SWTPM

##### Debian

```bash
sudo apt install swtpm
```

!!! note
    Als deze niet bestaat zoek je op welke linux versie je hebt.
    In ons geval is dit Zorin OS (focal)
    Dan voeg je eerst nog de repo toe.

```bash
sudo add-apt-repository ppa:stefanberger/swtpm-focal
sudo apt update**
sudo apt-get install swtpm swtpm-tools
```

##### Arch

```bash
sudo pacman -S swtpm
```

## Stap 3

Start vm nogmaals en stel deze nu in met volgende wijzigingen:

- Updates
- Fedora kvm guest tools

!!! note
    Een rechtstreekse link en klik op de .exe
    https://github.com/virtio-win/virtio-win-pkg-scripts/blob/master/README.md

Schakel de VM terug uit.

## Stap 4

- Clone de VM
- Share de disk als het mogelijk is met de nieuwe VM
- Geef de VM een naam "name"-gpu-passtrough

### Bewerk de VM

- [Add Hardware]
  - voeg nu alle USB en PCI(GPU) Ids toe.
- `Delete` "Display Spice, Serial 1, Channel Spice, and Video QXL"

### Install Libvirt

!!! note
    https://passthroughpo.st/simple-per-vm-libvirt-hooks-with-the-vfio-tools-hook-helper/

```bash
sudo mkdir -p /etc/libvirt/hooks
sudo wget 'https://raw.githubusercontent.com/PassthroughPOST/VFIO-Tools/master/libvirt_hooks/qemu' \ -O /etc/libvirt/hooks/qemu
sudo chmod +x /etc/libvirt/hooks/qemu
sudo service libvirtd restart
```

### GPU loskoppelen en terug koppelen

Nu ga je 2 schripts maken die de GPU gaan loskoppelen en terug koppelen aan je Linux device.
!!! note
Zet in de 2 scripts `pci_0000_01_00_0` naar jouw ID.

```bash
sudo mkdir -p /etc/libvirt/hooks/qemu.d/"VMNAME"/prepare/
sudo mkdir -p /etc/libvirt/hooks/qemu.d/"VMNAME"/prepare/begin
sudo nano /etc/libvirt/hooks/qemu.d/"VMNAME"/prepare/begin/script.sh
```


```bash
#!/bin/bash
# Helpful to read output when debugging
set -x

# Stop display manager
systemctl stop display-manager.service
## Uncomment the following line if you use GDM
killall gdm-x-session

# Unbind VTconsoles
echo 0 > /sys/class/vtconsole/vtcon0/bind
echo 0 > /sys/class/vtconsole/vtcon1/bind

# Unbind EFI-Framebuffer
echo efi-framebuffer.0 > /sys/bus/platform/drivers/efi-framebuffer/unbind

# Avoid a Race condition by waiting 2 seconds. This can be calibrated to be shorter or longer if required for your system
sleep 2

# Unbind the GPU from display driver
virsh nodedev-detach pci_0000_01_00_0
virsh nodedev-detach pci_0000_01_00_1

# Load VFIO Kernel Module
modprobe vfio-pci
```

```bash
sudo chmod +x /etc/libvirt/hooks/qemu.d/"VMNAME"/prepare/begin/script.sh
sudo mkdir -p /etc/libvirt/hooks/qemu.d/"VMNAME"/release/end
sudo nano /etc/libvirt/hooks/qemu.d/"VMNAME"/release/end/script.sh
```

```bash
#!/bin/bash
set -x

# Re-Bind GPU to Nvidia Driver
virsh nodedev-reattach pci_0000_01_00_1
virsh nodedev-reattach pci_0000_01_00_0

# Reload nvidia modules
modprobe nvidia
modprobe nvidia_modeset
modprobe nvidia_uvm
modprobe nvidia_drm

# Rebind VT consoles
echo 1 > /sys/class/vtconsole/vtcon0/bind
# Some machines might have more than 1 virtual console. Add a line for each corresponding VTConsole
echo 1 > /sys/class/vtconsole/vtcon1/bind

nvidia-xconfig --query-gpu-info > /dev/null 2>&1
echo "efi-framebuffer.0" > /sys/bus/platform/drivers/efi-framebuffer/bind

# Restart Display Manager
systemctl start display-manager.service
```

```bash
sudo chmod +x /etc/libvirt/hooks/qemu.d/win10/prepare/begin/script.sh
```

## Stap 5

Start VM
Enjoy :D

!!! note
https://www.youtube.com/watch?v=qaA7kxcnXaM
