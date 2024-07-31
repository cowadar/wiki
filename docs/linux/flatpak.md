# Pop!\_OS

## Pop!\_OS 20.04 has Flatpak installed and Flathub configured by default. The Pop!\_Shop can be used to install flatpaks

For older versions of Pop!\_OS, see the instructions below.

1. ## Install Flatpak

    To install Flatpak on Pop!\_OS 19.10 and earlier, simply run:

    ```bash
    sudo apt install flatpak
    ```

2. ## Add the Flathub repository

    Flathub is the best place to get Flatpak apps. To enable it, run:

    ```bash
    flatpak remote-add --user --if-not-exists flathub https://dl.flathub.org/repo/flathub.flatpakrepo
    ```

3. ## Restart

    To complete setup, restart your system. Now all you have to do is [install some apps](https://flathub.org/)!

    Note: graphical installation of Flatpak apps may not be possible with Pop!\_OS 19.10 and earlier.
