# Google Chrome

## Pop!_OS  installatie

!!! note
    Geen Chrome installeren via de Pop Store

1. Open terminal
2. Geef volgend commando in om de laatste stable versie te downloaden
    ```
    wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb
    ```
    !!! note
        Indien er libu2f-udev niet geinstalleerd is, kan je dit installeren met `sudo apt install libu2f-udev`
3. Geef volgend commando in om de file uit te voeren.
    ```bash
    sudo dpkg -i google-chrome-stable_current_amd64.deb
    ```
4. Klaar :)