## NFS Sharing

There are a few ways to mount a share from your unraid server on your ubuntu docker vm. One way is to use **SMB/CIFS** protocol and mount the share using **fstab** file. Another way is to use **NFS** protocol and mount the share using **/etc/fstab** file. A third way is to use **--cap-add** flag with docker run to enable **mount privileges** inside the container.

Here are some examples of how to do each method:

- SMB/CIFS with fstab:

```bash
# Install cifs-utils package
sudo apt-get install cifs-utils

# Create a mount point
sudo mkdir /mnt/unraid

# Edit /etc/fstab file and add a line like this
//<unraid_server_ip>/<share_name> /mnt/unraid cifs username=<unraid_user>,password=<unraid_password>,uid=<ubuntu_user>,gid=<ubuntu_group> 0 0

# Mount the share
sudo mount -a
```

- NFS with /etc/fstab:

```bash
# Install nfs-common package
sudo apt-get install nfs-common

# Create a mount point
sudo mkdir /mnt/unraid

# Edit /etc/fstab file and add a line like this
<unraid_server_ip>:/mnt/user/<share_name> /mnt/unraid nfs defaults,_netdev 0 0

# Mount the share
sudo mount -a
```

- --cap-add with docker run:

```bash
# Run the docker container with --cap-add SYS_ADMIN --cap-add DAC_READ_SEARCH flags
docker run -it --cap-add SYS_ADMIN --cap-add DAC_READ_SEARCH ubuntu bash

# Inside the container, install cifs-utils package
apt-get update && apt-get install cifs-utils

# Create a mount point
mkdir /mnt/unraid

# Mount the share using mount command
mount -t cifs //<unraid_server_ip>/<share_name> /mnt/unraid -o user=<unraid_user>,password=<unraid_password>
```
