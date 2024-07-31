---
title: Plex on Proxmox
---

## How to install Plex Media Server in a Proxmox Linux Container (LXC)

[Proxmox](https://www.proxmox.com/) containers are a lightweight alternative to fully Virtualized Machines [(VMs)](https://en.wikipedia.org/wiki/Virtual_machine). We will in this guide show how [Plex Media Server](https://www.plex.tv/) can be installed in a [Linux Container (LXC)](https://pve.proxmox.com/wiki/Linux_Container).

## Introducing Linux Containers (LXC)

Linux Containers (LXC) use the [kernel](https://en.wikipedia.org/wiki/Kernel_(operating_system)) of the host system instead of emulating a full [Operating System (OS)](https://en.wikipedia.org/wiki/Operating_system). This means that containers can access resources on the host system directly and at much lower memory cost. As an example, Plex Media Server running in a VM can easily fetch several Gigabytes of RAM, while in a container, the same installation uses only a few of hundred Megabytes.

### Privileged vs Unprivileged

LXC Containers can be of two kinds; [privileged](https://linuxcontainers.org/lxc/security/) and [unprivileged](https://linuxcontainers.org/lxc/security/). In a privileged container you will be able to modify resources that exist _outside_ the container, while in an unprivileged container, it is only possible to modify external resources once the host has given its explicit permission.

We will in this tutorial cover **privileged containers** only due to their ease of use and by assuming that your Proxmox environment resides on a non-public facing, trusted [Local Area Network (LAN)](https://en.wikipedia.org/wiki/Local_area_network). If you are concerned about security; use unprivileged containers, but be aware that you _cannot_ delete any _external_ files from inside the Plex Media Server container\*.

_Based on the information provided by this guide._

___

## Software Versions Used

At the time of writing, the following software versions were used:

-   [Proxmox VE 7.3-3](https://www.proxmox.com/en/downloads/item/proxmox-ve-7-3-iso-installer)
-   [Debian 11 Bullseye (standard) 11.3-1](http://download.proxmox.com/images/system/)

We have further assumed that Plex Media Server will be installed on a default Proxmox configuration with one node and one hard drive.

___

## Templates

With the formalities out of the way, let’s go ahead and download our first LXC Template.

Pick a storage area for the container. We have chosen the local disk; **local (pve)** \[1\] and then selected **CT Templates** \[2\], followed by the **Templates** \[3\] button.

![Plex in Proxmox LXC Container - Image 1](https://www.geekbitzone.com/posts/2022/proxmox/plex-lxc/img/install-plex-in-proxmox-lxc-1.png)

### Debian

Plex Media Server can run on many platforms, but we prefer [Debian](https://www.debian.org/), and more specifically the **debian-11-standard 11.3-1** \[1\] template, because it is stable, secure and the base for many other Linux distributions, such as [Ubuntu](https://ubuntu.com/).

Once you have selected the template, press **Download** \[2\].

![Plex in Proxmox LXC Container - Image 2](https://www.geekbitzone.com/posts/2022/proxmox/plex-lxc/img/install-plex-in-proxmox-lxc-2.png)

**Close** the Task Viewer once the download is complete.

![Plex in Proxmox LXC Container - Image 3](https://www.geekbitzone.com/posts/2022/proxmox/plex-lxc/img/install-plex-in-proxmox-lxc-3.png)

### Creating the Container

We are now ready to create the LXC Container. Press the **Create CT** button in the upper-right corner.

![Plex in Proxmox LXC Container - Image 4](https://www.geekbitzone.com/posts/2022/proxmox/plex-lxc/img/install-plex-in-proxmox-lxc-4.png)

Set **CT ID** \[1\] to a number of your choice\*. We have chosen `100` and given our **Hostname** \[2\] the name `plex`.

If you want the option to be able to delete external files from within Plex Media Server, set **Unprivileged container** \[3\] to `off`, i.e. make it a _**Privileged**_ container.

Finally, enter and confirm your super-secret **Password** \[4\]\[5\].

Press **Next** \[6\].

\*_Container numbers start from 100 to “infinite” and **cannot** overlap with existing containers or VMs._

![Plex in Proxmox LXC Container - Image 5](https://www.geekbitzone.com/posts/2022/proxmox/plex-lxc/img/install-plex-in-proxmox-lxc-5.png)

Select `debian-11-standard_11.3-1_amd64.tar.zst` (the file that you just downloaded), from the **Template** \[1\] list.

Press **Next** \[2\].

![Plex in Proxmox LXC Container - Image 6](https://www.geekbitzone.com/posts/2022/proxmox/plex-lxc/img/install-plex-in-proxmox-lxc-6.png)

_Optional:_ Adjust **Disk size** if you intend store your media inside the LXC Container (not recommended), or if you need more space for metadata and thumbnails.

Press **Next**.

![Plex in Proxmox LXC Container - Image 7](https://www.geekbitzone.com/posts/2022/proxmox/plex-lxc/img/install-plex-in-proxmox-lxc-7.png)

_Optional:_ Set the number of CPU **Cores**. We are just using `1` core in this example, but adjust higher if you think you will need more power.

Press **Next**.

![Plex in Proxmox LXC Container - Image 8](https://www.geekbitzone.com/posts/2022/proxmox/plex-lxc/img/install-plex-in-proxmox-lxc-8.png)

_Optional:_ The Plex container can run with only `512` MiB RAM, but adjust here if you think more memory will be required.

Press **Next**.

![Plex in Proxmox LXC Container - Image 9](https://www.geekbitzone.com/posts/2022/proxmox/plex-lxc/img/install-plex-in-proxmox-lxc-9.png)

We will access the Plex Media Server interface via **IPv4 DHCP** \[1\], but you can also leave it at static if you do not have the option (or do not want) to create a [Static DHCP](https://www.howtogeek.com/69612/how-to-set-up-static-dhcp-on-your-dd-wrt-router/) reservation on your [router](https://en.wikipedia.org/wiki/Router_(computing)).

Press **Next** \[2\].

![Plex in Proxmox LXC Container - Image 10](https://www.geekbitzone.com/posts/2022/proxmox/plex-lxc/img/install-plex-in-proxmox-lxc-10.png)

We will let our router handle [DNS](https://en.wikipedia.org/wiki/Domain_Name_System) queries, but adjust here if necessary.

Press **Next**.

![Plex in Proxmox LXC Container - Image 11](https://www.geekbitzone.com/posts/2022/proxmox/plex-lxc/img/install-plex-in-proxmox-lxc-11.png)

Confirm that all settings are okay and tick **Start after created** \[1\].

Press **Finish** \[2\].

![Plex in Proxmox LXC Container - Image 12](https://www.geekbitzone.com/posts/2022/proxmox/plex-lxc/img/install-plex-in-proxmox-lxc-12.png)

**Close** the Task Viewer once confirmed that the container has been created.

![Plex in Proxmox LXC Container - Image 13](https://www.geekbitzone.com/posts/2022/proxmox/plex-lxc/img/install-plex-in-proxmox-lxc-13.png)

Your LXC Container should now be ready and up and running.

___

## Configuration

We are now ready to log in to the container.

Navigate to **100 (plex)** \[1\] and select **Console** \[2\].

![Plex in Proxmox LXC Container - Image 14](https://www.geekbitzone.com/posts/2022/proxmox/plex-lxc/img/install-plex-in-proxmox-lxc-14.png)

**Login** as `root`, followed by your `super-secret` **Password**.

![Plex in Proxmox LXC Container - Image 15](https://www.geekbitzone.com/posts/2022/proxmox/plex-lxc/img/install-plex-in-proxmox-lxc-15.png)

### Updating the System

There is a good chance the default software packages that came with the Debian 11 template are out-of-date. We will therefore update the [APT](https://en.wikipedia.org/wiki/APT_(software)) repository to ensure we have the latest list of packages.

In the terminal window, type:

![Plex in Proxmox LXC Container - Image 16](https://www.geekbitzone.com/posts/2022/proxmox/plex-lxc/img/install-plex-in-proxmox-lxc-16.png)

Followed by the upgrade command:

![Plex in Proxmox LXC Container - Image 17](https://www.geekbitzone.com/posts/2022/proxmox/plex-lxc/img/install-plex-in-proxmox-lxc-17.png)

### Installing Plex

Plex Media Server is not included, by default, in Debian’s APT repositories, which means that it will have to be downloaded separately from [Plex.tv](https://www.plex.tv/)’s servers.

We will first, however, need to install [curl](https://curl.se/) to enable fetching of URLs and [gnupg](https://gnupg.org/) for secure authentication:

```
apt install curl gnupg -y
```

![Plex in Proxmox LXC Container - Image 18](https://www.geekbitzone.com/posts/2022/proxmox/plex-lxc/img/install-plex-in-proxmox-lxc-18.png)

To install Plex, a repository file must be created that pulls directly from the Plex repository. But first, a GPG key must be imported with the `curl` command and saved under `/usr/share/keyrings/plex.gpg`.

```
curl -sS https://downloads.plex.tv/plex-keys/PlexSign.key | gpg --dearmor | tee /usr/share/keyrings/plex.gpg > /dev/null
```

![Plex in Proxmox LXC Container - Image 19](https://www.geekbitzone.com/posts/2022/proxmox/plex-lxc/img/install-plex-in-proxmox-lxc-19.png)

Next, add the Plex repository to the standard APT sources directory.

```
echo "deb [signed-by=/usr/share/keyrings/plex.gpg] https://downloads.plex.tv/repo/deb public main" > /etc/apt/sources.list.d/plexmediaserver.list
```

![Plex in Proxmox LXC Container - Image 20](https://www.geekbitzone.com/posts/2022/proxmox/plex-lxc/img/install-plex-in-proxmox-lxc-20.png)

Plex Media Server is now first class citizen in our APT repository, which means that we can go ahead and install the package as normal with the following command:

```
apt update && apt install plexmediaserver -y
```

![Plex in Proxmox LXC Container - Image 21](https://www.geekbitzone.com/posts/2022/proxmox/plex-lxc/img/install-plex-in-proxmox-lxc-21.png)

_Optional:_ **Reboot** the container to ensure that any currently locked process are re-initialised after the package update.

![Plex in Proxmox LXC Container - Image 22](https://www.geekbitzone.com/posts/2022/proxmox/plex-lxc/img/install-plex-in-proxmox-lxc-22.png)

Log back in with your credentials.

![Plex in Proxmox LXC Container - Image 15](https://www.geekbitzone.com/posts/2022/proxmox/plex-lxc/img/install-plex-in-proxmox-lxc-15.png)

Verify that Plex is **active (running)** with the `systemctl` command:

```
systemctl status plexmediaserver
```

![Plex in Proxmox LXC Container - Image 23](https://www.geekbitzone.com/posts/2022/proxmox/plex-lxc/img/install-plex-in-proxmox-lxc-23.png)

The eagle-eyed user might see the following, rather worrisome, error message:

`Critical: libusb_init failed`

After some Google-Fu:ing we ended up on the following [Reddit](https://www.reddit.com/r/docker/comments/j0klle/critical_libusb_init_failed_what_does_this_mean/) page which provided an explanation as to why the error is shown, but the suggested solution did not work in our case. [libusb](https://libusb.info/) is, however, only used for Plex’s DVR functionality, and should _not_ affect standard movie watching. If you do have a work-around, please post it in the comments below, and we will update this article.

(Press `q` on the keyboard to exit this screen.)

### Logging into Plex

The Plex Media Server service is now up and running and we should be able to launch a web browser to access its web interface.

First we need to find out the IP address for our server:

![Plex in Proxmox LXC Container - Image 24](https://www.geekbitzone.com/posts/2022/proxmox/plex-lxc/img/install-plex-in-proxmox-lxc-24.png)

Our container’s IP address resolves to `192.168.5.206`, so we will use that, followed by Plex’s standard port number, `32400`, and `/web` directory when typing the URL in our browser:

```
https://<your_plex_server_url>:32400/web
```

![Plex in Proxmox LXC Container - Image 25](https://www.geekbitzone.com/posts/2022/proxmox/plex-lxc/img/install-plex-in-proxmox-lxc-25.png)

If you typed in the correct address, you will be automatically redirected to [https://app.plex.tv](https://app.plex.tv/) for login authentication.

Go ahead and sign in with your **Username** and **Password** and then press the **Sign In** button.

![Plex in Proxmox LXC Container - Image 26](https://www.geekbitzone.com/posts/2022/proxmox/plex-lxc/img/install-plex-in-proxmox-lxc-26.png)

Yep, we **Got It!**

![Plex in Proxmox LXC Container - Image 27](https://www.geekbitzone.com/posts/2022/proxmox/plex-lxc/img/install-plex-in-proxmox-lxc-27.png)

Give your server a friendly **Name** \[1\], such as `plex`.

_Optional:_ We will **not** allow Plex to automatically configure our network for outside access as there are much better (and safer) ways to access your home network, such as through a VPN.

Press **Next** \[2\].

![Plex in Proxmox LXC Container - Image 28](https://www.geekbitzone.com/posts/2022/proxmox/plex-lxc/img/install-plex-in-proxmox-lxc-28.png)

### Adding Media to Plex

We are now ready to add our legally downloaded media files to Plex.

Press **Add Library**.

![Plex in Proxmox LXC Container - Image 29](https://www.geekbitzone.com/posts/2022/proxmox/plex-lxc/img/install-plex-in-proxmox-lxc-29.png)

Let’s start with some **Films**.

Press **Next**.

![Plex in Proxmox LXC Container - Image 30](https://www.geekbitzone.com/posts/2022/proxmox/plex-lxc/img/install-plex-in-proxmox-lxc-30.png)

To add your movies, Press the **Browse for Media Folder** button.

![Plex in Proxmox LXC Container - Image 31](https://www.geekbitzone.com/posts/2022/proxmox/plex-lxc/img/install-plex-in-proxmox-lxc-31.png)

Wait a minute! _Where_ are my movie files?!

![Plex in Proxmox LXC Container - Image 32](https://www.geekbitzone.com/posts/2022/proxmox/plex-lxc/img/install-plex-in-proxmox-lxc-32.png)

Unless you are storing your entire movie library inside the LXC Container _(not recommended)_, you will need to find a way to access your files from _outside_ the container. There are several ways that this can be done.

If your files reside on a [Network Attached Storage (NAS)](https://en.wikipedia.org/wiki/Network-attached_storage), it is common to access them via the [SMB/CIFS](https://en.wikipedia.org/wiki/Server_Message_Block) or [NFS](https://en.wikipedia.org/wiki/Network_File_System) protocols.

If the files are in a directory on the Proxmox host, they can be directly mounted in the host onto the container.

We will in the next three sections explain the most common methods for how files can be accessed from inside the LXC Container. Please follow along with the option that applies to your specific network setup.

-   [Option 1: Mounting an SMB/CIFS share](https://www.geekbitzone.com/posts/2022/proxmox/plex-lxc/install-plex-in-proxmox-lxc/#Option1_Mounting_SMB-CIFS_Share)
-   [Option 2: Mounting an NFS Share](https://www.geekbitzone.com/posts/2022/proxmox/plex-lxc/install-plex-in-proxmox-lxc/#Option2_Mounting_NFS_Share)
-   [Option 3: Mounting a directory on the Proxmox host](https://www.geekbitzone.com/posts/2022/proxmox/plex-lxc/install-plex-in-proxmox-lxc/#Option3_Mounting_Host_Directory)

___

The SMB/CIFS protocol provides support for cross-platform file sharing with Microsoft Windows, OS X, and other Unix systems.

### Installing cifs-utils

Let’s return to the container’s console and install the [cifs-utils](https://packages.debian.org/stable/otherosfs/cifs-utils) package.

```
apt install cifs-utils -y
```

![Plex in Proxmox LXC Container - Image 33](https://www.geekbitzone.com/posts/2022/proxmox/plex-lxc/img/install-plex-in-proxmox-lxc-33.png)

### Creating CIFS Credentials

Once cifs-utils have been downloaded, create an empty text file and place it under `/root/.cifs_credentials`. We prefer [nano](https://www.nano-editor.org/), but use whichever text editor you are comfortable with.

```
nano /root/.cifs_credentials
```

![Plex in Proxmox LXC Container - Image 34](https://www.geekbitzone.com/posts/2022/proxmox/plex-lxc/img/install-plex-in-proxmox-lxc-34.png)

Inside the file, add your SMB/CIFS **username=**`<username>` on the first line and **password=**`<password>` on the second line.

_Tip: (**Ctrl+o** saves the file and **Ctrl+x** exits the editor.)_

```
username=<username>
password=<password>
```

![Plex in Proxmox LXC Container - Image 35](https://www.geekbitzone.com/posts/2022/proxmox/plex-lxc/img/install-plex-in-proxmox-lxc-35.png)

_Optional:_ Increase security by adjusting the read/write permissions on the file to the current owner only.

```
chmod 600 /root/.cifs_credentials
```

![Plex in Proxmox LXC Container - Image 36](https://www.geekbitzone.com/posts/2022/proxmox/plex-lxc/img/install-plex-in-proxmox-lxc-36.png)

### Creating the Mount Directory

Create a directory in which we will mount our SMB/CIFS share. As we are using Plex, let’s give the directory the appropriate name;`/mnt/plex`.

![Plex in Proxmox LXC Container - Image 37](https://www.geekbitzone.com/posts/2022/proxmox/plex-lxc/img/install-plex-in-proxmox-lxc-37.png)

### Retrieving Plex uid and gid

_This step is necessary if you want to allow deletion of media files from within Plex Media Server._

When a directory is mounted it will belong to the user who first mounted it. In our example, the owner is `root`, while Plex Media Server runs as user `plex`. It will therefore not be able to modify any files belonging to `root` unless ownership is changed during the mount process.

In order to do so we need to first retrieve the **User ID** (`uid`) and **Group ID** (`gid`) of user `plex`. Execute the following command and write these numbers down because they will be used in the next section:

![Plex in Proxmox LXC Container - Image 38](https://www.geekbitzone.com/posts/2022/proxmox/plex-lxc/img/install-plex-in-proxmox-lxc-38.png)

### Systemd

We will mount our network share with [systemd](https://en.wikipedia.org/wiki/Systemd), rather than [fstab](https://en.wikipedia.org/wiki/Fstab), because the latter does not seem to remount on reboot as discussed in this [Reddit](https://www.reddit.com/r/Proxmox/comments/w3rkf4/mounting_a_cifs_share_in_a_privileged_lxc/) post.

Under `/etc/systemd/system/`, create a new [unit configuration file (.mount)](https://www.freedesktop.org/software/systemd/man/systemd.mount.html) that inherits its name from the full path to your mounted directory, with the quirk that all forward slashes `/` are replaced with a hyphen `-`.

_Example 1:_ Your plex directory resides under _**/mnt/plex**_. Your unit configuration file should therefore be called `/etc/systemd/system/mnt-plex.mount`.

_Example 2:_ A randomly shared folder under _**/home/geek/Public/important\_files**_ must be called `/etc/systemd/system/home-geek-Public-important_files.mount` and **nothing** else!

Armed with this information, let’s go ahead and create the file:

```
nano /etc/systemd/system/<directory-with-hyphen>.mount
```

![Plex in Proxmox LXC Container - Image 39](https://www.geekbitzone.com/posts/2022/proxmox/plex-lxc/img/install-plex-in-proxmox-lxc-39.png)

Add the following information and replace `<server_path>`, `<share>`, `<mount_point>`, `<uid>` and `<gid>` as necessary.

Save and close the file.

```
[Unit]
  Description=Plex Media Server files
  Requires=network-online.target
  After=network-online.service

[Mount]
  What=//<server_path>/<share>
  Where=/<mount_point>
  Options=credentials=/root/.cifs_credentials,uid=<uid>,gid=<gid>
  Type=cifs

[Install]
  WantedBy=multi-user.target
```

![Plex in Proxmox LXC Container - Image 40](https://www.geekbitzone.com/posts/2022/proxmox/plex-lxc/img/install-plex-in-proxmox-lxc-40.png)

Enable the unit configuration file:

```
systemctl enable <directory-with-hyphen>.mount
```

![Plex in Proxmox LXC Container - Image 41](https://www.geekbitzone.com/posts/2022/proxmox/plex-lxc/img/install-plex-in-proxmox-lxc-41.png)

Start the directory service to automatically mount the folder:

```
systemctl start <directory-with-hyphen>.mount
```

![Plex in Proxmox LXC Container - Image 42](https://www.geekbitzone.com/posts/2022/proxmox/plex-lxc/img/install-plex-in-proxmox-lxc-42.png)

Verify that the files are now seen in the mounted directory.

![Plex in Proxmox LXC Container - Image 43](https://www.geekbitzone.com/posts/2022/proxmox/plex-lxc/img/install-plex-in-proxmox-lxc-43.png)

Great, the files are there, and we can now go ahead and add this folder to Plex Media Server in [Completing the Setup](https://www.geekbitzone.com/posts/2022/proxmox/plex-lxc/install-plex-in-proxmox-lxc/#Completing_the_Setup).

If you are using NFS shares instead, read on for [Option 2: Mounting an NFS Share](https://www.geekbitzone.com/posts/2022/proxmox/plex-lxc/install-plex-in-proxmox-lxc/#Option2_Mounting_NFS_Share).

___

_Caution:_ If you are continuing from [Option 1: Mounting an SMB/CIFS share](https://www.geekbitzone.com/posts/2022/proxmox/plex-lxc/install-plex-in-proxmox-lxc/#Option1_Mounting_SMB-CIFS_Share), please reset the container first and restart the tutorial from the [Configuration](https://www.geekbitzone.com/posts/2022/proxmox/plex-lxc/install-plex-in-proxmox-lxc/#Configuration) section. Your mounted directory will otherwise be mapped twice with different protocols!

___

Network File System (NFS) is a distributed file system, which in comparison to SMB/CIFS, has better performance when medium-sized or small files are accessed.

If your LXC Container is still running, please turn it off with the **Shutdown** button.

![Plex in Proxmox LXC Container - Image 44](https://www.geekbitzone.com/posts/2022/proxmox/plex-lxc/img/install-plex-in-proxmox-lxc-44.png)

### Enabling NFS Container Support

Assuming that you are using a _Privileged_ container, click on **100 (plex)** \[1\], then **Options** \[2\] and _double-click_ on **Features** \[3\].

![Plex in Proxmox LXC Container - Image 45](https://www.geekbitzone.com/posts/2022/proxmox/plex-lxc/img/install-plex-in-proxmox-lxc-45.png)

Enable **NFS** \[1\] and press **OK** \[2\].

![Plex in Proxmox LXC Container - Image 46](https://www.geekbitzone.com/posts/2022/proxmox/plex-lxc/img/install-plex-in-proxmox-lxc-46.png)

**Features** should now display `mount=nfs`.

![Plex in Proxmox LXC Container - Image 47](https://www.geekbitzone.com/posts/2022/proxmox/plex-lxc/img/install-plex-in-proxmox-lxc-47.png)

Go back to **100 (plex)** \[1\], **Console** \[2\] and **Start** \[3\] the container.

![Plex in Proxmox LXC Container - Image 48](https://www.geekbitzone.com/posts/2022/proxmox/plex-lxc/img/install-plex-in-proxmox-lxc-48.png)

Log back in with your credentials.

![Plex in Proxmox LXC Container - Image 15](https://www.geekbitzone.com/posts/2022/proxmox/plex-lxc/img/install-plex-in-proxmox-lxc-15.png)

### Installing nfs-common

Once logged in, install the [nfs-common](https://packages.debian.org/bullseye/nfs-common) package.

```
apt install nfs-common -y
```

![Plex in Proxmox LXC Container - Image 49](https://www.geekbitzone.com/posts/2022/proxmox/plex-lxc/img/install-plex-in-proxmox-lxc-49.png)

### Creating the Mount Directory

Create a directory in which we will mount our NFS share. As we are using Plex, let’s give the directory the appropriate name;`/mnt/plex`.

![Plex in Proxmox LXC Container - Image 37](https://www.geekbitzone.com/posts/2022/proxmox/plex-lxc/img/install-plex-in-proxmox-lxc-37.png)

### Adding fstab Entries

NFS shares in LXC Containers can be mounted via `/etc/fstab`, so we will go ahead and edit this file:

![Plex in Proxmox LXC Container - Image 50](https://www.geekbitzone.com/posts/2022/proxmox/plex-lxc/img/install-plex-in-proxmox-lxc-50.png)

Add the following information by replacing `<server_path>`, `<share>` and `<directory>` with your own network share settings and then save and close the file.

```
<server_path>:/<share>       /<directory>       nfs     defaults        0       0
```

![Plex in Proxmox LXC Container - Image 51](https://www.geekbitzone.com/posts/2022/proxmox/plex-lxc/img/install-plex-in-proxmox-lxc-51.png)

### Mounting the Volume

We can now mount the directory in _/etc/fstab_ with the `mount -a` command.

![Plex in Proxmox LXC Container - Image 52](https://www.geekbitzone.com/posts/2022/proxmox/plex-lxc/img/install-plex-in-proxmox-lxc-52.png)

Verify that the files are now seen in the mounted directory.

![Plex in Proxmox LXC Container - Image 43](https://www.geekbitzone.com/posts/2022/proxmox/plex-lxc/img/install-plex-in-proxmox-lxc-43.png)

Yep, the files can be seen and the directory will remain mounted even after a reboot.

You can now jump to [Completing the Setup](https://www.geekbitzone.com/posts/2022/proxmox/plex-lxc/install-plex-in-proxmox-lxc/#Completing_the_Setup) or read on for [Option 3: Mounting a Directory on the Proxmox host](https://www.geekbitzone.com/posts/2022/proxmox/plex-lxc/install-plex-in-proxmox-lxc/#Option3_Mounting_Host_Directory).

___

_Caution:_ If you are continuing from [Option 2: Mounting an NFS Share](https://www.geekbitzone.com/posts/2022/proxmox/plex-lxc/install-plex-in-proxmox-lxc/#Option2_Mounting_NFS_Share), please reset the container first and restart the tutorial from the [Configuration](https://www.geekbitzone.com/posts/2022/proxmox/plex-lxc/install-plex-in-proxmox-lxc/#Configuration) section. Your mounted directory will otherwise be mapped twice with different protocols!

___

## Option 3: Mounting a Directory on the Proxmox host

The final option that we will discuss is how to mount an LXC container directory directly from within the Proxmox host.

If your LXC Container is still running, please turn it off with the **Shutdown** button.

![Plex in Proxmox LXC Container - Image 44](https://www.geekbitzone.com/posts/2022/proxmox/plex-lxc/img/install-plex-in-proxmox-lxc-44.png)

We will now open a terminal directly on the Proxmox host. _(Note: **not** on the container)_.

Select **pve** \[1\] and open up a new **Shell** \[2\].

![Plex in Proxmox LXC Container - Image 53](https://www.geekbitzone.com/posts/2022/proxmox/plex-lxc/img/install-plex-in-proxmox-lxc-53.png)

### The pct Command

Let’s assume that our media files are stored in the following directory on the Proxmox host, `/mnt/plex`.

We can now tell Proxmox to mirror this local directory onto the LXC Container with this simple [Proxmox Container Toolkit](https://pve.proxmox.com/pve-docs/chapter-pct.html) (`pct`) command:

Replace `<id>`, `<source>` and `<target>` with your own specific parameters.

```
pct set <id> -mp0 <source>,mp=<target>
```

![Plex in Proxmox LXC Container - Image 54](https://www.geekbitzone.com/posts/2022/proxmox/plex-lxc/img/install-plex-in-proxmox-lxc-54.png)

### Changing File Ownership

If you want the option to be able to delete external files from within Plex Media Server, change the ownership of the mount directory to `plex`.

_Note: We are currently uncertain whether this is the correct way to change ownership for directories inside LXC Containers. If you aware of a “better” method, please let us know in the comments below and we will update this section. For now, it seems to work for what we are trying to do._

```
chown -R plex:plex /mnt/plex/
```

![Plex in Proxmox LXC Container - Image 55](https://www.geekbitzone.com/posts/2022/proxmox/plex-lxc/img/install-plex-in-proxmox-lxc-55.png)

### Launching the Container

Let’s exit our Proxmox shell and return to the web interface and select **100 (plex)** \[1\], **Resources** \[2\] to verify that the **Mount Point (mp0)** \[3\] has been set to `/mnt/plex,mp=/mnt/plex`.

Good, it is there.

![Plex in Proxmox LXC Container - Image 56](https://www.geekbitzone.com/posts/2022/proxmox/plex-lxc/img/install-plex-in-proxmox-lxc-56.png)

Go back to **100 (plex)** \[1\], **Console** \[2\] and **Start** \[3\] the container.

![Plex in Proxmox LXC Container - Image 48](https://www.geekbitzone.com/posts/2022/proxmox/plex-lxc/img/install-plex-in-proxmox-lxc-48.png)

Verify that the files are now seen in the mounted directory.

![Plex in Proxmox LXC Container - Image 43](https://www.geekbitzone.com/posts/2022/proxmox/plex-lxc/img/install-plex-in-proxmox-lxc-43.png)

Great, the files are there, and we can now continue to [Completing the Setup](https://www.geekbitzone.com/posts/2022/proxmox/plex-lxc/install-plex-in-proxmox-lxc/#Completing_the_Setup).

___

## Completing the Setup

Select your media folder and press **Add**.

![Plex in Proxmox LXC Container - Image 57](https://www.geekbitzone.com/posts/2022/proxmox/plex-lxc/img/install-plex-in-proxmox-lxc-57.png)

Once you have finished adding the folders, press **Add Library**.

![Plex in Proxmox LXC Container - Image 58](https://www.geekbitzone.com/posts/2022/proxmox/plex-lxc/img/install-plex-in-proxmox-lxc-58.png)

When you are done, press **Next**.

![Plex in Proxmox LXC Container - Image 59](https://www.geekbitzone.com/posts/2022/proxmox/plex-lxc/img/install-plex-in-proxmox-lxc-59.png)

If you need to get some Plex Apps, do that here and then press **Done**.

![Plex in Proxmox LXC Container - Image 60](https://www.geekbitzone.com/posts/2022/proxmox/plex-lxc/img/install-plex-in-proxmox-lxc-60.png)

Press **Finish Setup**.

![Plex in Proxmox LXC Container - Image 61](https://www.geekbitzone.com/posts/2022/proxmox/plex-lxc/img/install-plex-in-proxmox-lxc-61.png)

Congratulations! Plex Media Server is now up and running in the LXC Container, and it has also full access to the files on your network.

___

## Summary

Proxmox containers are a lightweight alternative to fully Virtualized Machines VMs. This guide has shown how Plex Media Server can be installed in a Linux Container (LXC) and how external files can be accessed from within the container.

___

## References

**Proxmox** [https://www.proxmox.com](https://www.proxmox.com/)

**Virtualized Machine (VM)** [https://en.wikipedia.org/wiki/Virtual\_machine](https://en.wikipedia.org/wiki/Virtual_machine))

**Plex Media Server** [https://www.plex.tv](https://www.plex.tv/)

**Linux Container (LXC)** [https://pve.proxmox.com/wiki/Linux\_Container](https://pve.proxmox.com/wiki/Linux_Container)

**Kernel (Operating System)** [https://en.wikipedia.org/wiki/Kernel\_(operating\_system)](https://en.wikipedia.org/wiki/Kernel_(operating_system))

**Operating System (OS)** [https://en.wikipedia.org/wiki/Operating\_system](https://en.wikipedia.org/wiki/Operating_system)

**Privileged and Unprivileged LXC Containers** [https://linuxcontainers.org/lxc/security/](https://linuxcontainers.org/lxc/security/)

**Local Area Network (LAN)** [https://en.wikipedia.org/wiki/Local\_area\_network](https://en.wikipedia.org/wiki/Local_area_network)

**Proxmox VE 7.3-3** [https://www.proxmox.com/en/downloads/item/proxmox-ve-7-3-iso-installer](https://www.proxmox.com/en/downloads/item/proxmox-ve-7-3-iso-installer)

**Debian 11 Bullseye (standard) 11.3-1** [http://download.proxmox.com/images/system/](http://download.proxmox.com/images/system/)

**Debian.org** [https://www.debian.org](https://www.debian.org/)

**Ubuntu** [https://ubuntu.com](https://ubuntu.com/)

**How to Set Up Static DHCP So Your Computer’s IP Address Does Not Change** [https://www.howtogeek.com/69612/how-to-set-up-static-dhcp-on-your-dd-wrt-router/](https://www.howtogeek.com/69612/how-to-set-up-static-dhcp-on-your-dd-wrt-router/)

**Router (Computing)** [https://en.wikipedia.org/wiki/Router\_(computing)](https://en.wikipedia.org/wiki/Router_(computing))

**Domain Name System (DNS)** [https://en.wikipedia.org/wiki/Domain\_Name\_System](https://en.wikipedia.org/wiki/Domain_Name_System)

**Advance Package Tool (APT)** [https://en.wikipedia.org/wiki/APT\_(software)](https://en.wikipedia.org/wiki/APT_(software))

**Curl** [https://curl.se](https://curl.se/)

**GnuPG** [https://gnupg.org](https://gnupg.org/)

**Libusb** [https://libusb.info](https://libusb.info/)

**Network Attached Storage (NAS)** [https://en.wikipedia.org/wiki/Network-attached\_storage](https://en.wikipedia.org/wiki/Network-attached_storage)

**SMB/CIFS** [https://en.wikipedia.org/wiki/Server\_Message\_Block](https://en.wikipedia.org/wiki/Server_Message_Block)

**NFS** [https://en.wikipedia.org/wiki/Network\_File\_System](https://en.wikipedia.org/wiki/Network_File_System)

**cifs-utils** [https://packages.debian.org/stable/otherosfs/cifs-utils](https://packages.debian.org/stable/otherosfs/cifs-utils)

**nano** [https://www.nano-editor.org](https://www.nano-editor.org/))

**systemd** [https://en.wikipedia.org/wiki/Systemd](https://en.wikipedia.org/wiki/Systemd)

**fstab** [https://en.wikipedia.org/wiki/Fstab](https://en.wikipedia.org/wiki/Fstab))

**Unit Configuration File (.mount)** [https://www.freedesktop.org/software/systemd/man/systemd.mount.html](https://www.freedesktop.org/software/systemd/man/systemd.mount.html)

**nfs-common** [https://packages.debian.org/bullseye/nfs-common](https://packages.debian.org/bullseye/nfs-common)

**Proxmox Container Toolkit (pct)** [https://pve.proxmox.com/pve-docs/chapter-pct.html](https://pve.proxmox.com/pve-docs/chapter-pct.html)