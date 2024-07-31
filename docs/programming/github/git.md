# Git

Git is een snel, schaalbaar, gedistribueerd versiebeheersysteem met een ongewoon rijke commandoset die zowel hoog-niveau operaties als volledige toegang tot de interne werking biedt. Git commando is het hoofdcommando dat wordt gebruikt om andere git subcommando’s aan te roepen. De algemene syntax voor het commando is: `git <opties> <subcommando> [<argumenten>]`. Enkele veelgebruikte subcommando’s zijn:

- [config](git.md#git-config) om de gebruiker te configureren of globale instellingen te wijzigen.
- [init](git.md#git-init) om een lokale repository te maken.
- `clone` om een kopie te maken van een bestaande repository van een URL.
- `add` om bestanden toe te voegen aan de staging area.
- `commit` om een snapshot te maken van de wijzigingen en deze op te slaan in de git directory.
- `status` om de lijst van gewijzigde bestanden en de bestanden die nog moeten worden gestaged of gecommit te tonen.
- `push` om lokale commits naar de externe repository te sturen.
- `pull` om wijzigingen van de externe repository op te halen en samen te voegen met de lokale branch.
- `branch` om branches te lijsten, maken, hernoemen of verwijderen.
- `checkout` om branches aan te maken en ertussen te wisselen of bestanden uit te checken.
- `merge` om twee of meer branches samen te voegen tot één branch.
- `log` om de geschiedenis van commits te tonen.

## Git config

`Git config` is een subcommando dat wordt gebruikt om git configuratie waarden in te stellen op een globaal of lokaal projectniveau. Deze configuratieniveaus komen overeen met .gitconfig tekstbestanden. Het uitvoeren van git config zal een configuratietekstbestand wijzigen. Je kunt met dit commando je persoonlijke werkvoorkeuren instellen, zoals je e-mail, gebruikersnaam, bestandsformaat, kleuren, enzovoort.

De algemene syntax voor het commando is: `git config <opties> <naam> [<waarde>]`. Enkele veelgebruikte opties zijn:

- `global` om een waarde in te stellen voor alle lokale repositories. De waarde wordt opgeslagen in het bestand ~/.gitconfig (of ~/.config/git/config).
- `local` om een waarde in te stellen voor de huidige repository. De waarde wordt opgeslagen in het bestand .git/config. Dit is het standaardniveau als er geen optie wordt gegeven.
- `system` om een waarde in te stellen voor alle gebruikers op het systeem en al hun repositories. De waarde wordt opgeslagen in het bestand [path]/etc/gitconfig. Je moet de optie --system expliciet geven om van dit niveau te lezen of te schrijven.
- `file <bestand>` om een waarde in te stellen in een specifiek bestand dat je opgeeft.
- `add` om een nieuwe regel toe te voegen aan een optie die meerdere regels kan hebben.
- `replace-all` om alle regels die overeenkomen met de naam (en eventueel de waarde-patroon) te vervangen door een nieuwe regel.
- `get` om de waarde van een bepaalde naam te tonen.
- `get-all` om alle waarden van een bepaalde naam te tonen.
- `get-regexp` om alle namen en waarden die overeenkomen met een reguliere expressie te tonen.
- `unset` om een naam (en eventueel de waarde-patroon) te verwijderen.
- `unset-all` om alle namen (en eventueel de waarde-patronen) die overeenkomen met de reguliere expressie te verwijderen.
- `rename-section` om een sectie te hernoemen naar een nieuwe naam.
- `remove-section` om een sectie en al zijn waarden te verwijderen.
- `l` of `--list` om alle namen en waarden van de configuratiebestanden te tonen.
- `-e` of `edit` om het configuratiebestand met je editor te openen en aan te passen.

## Git init

`Git init` is een subcommando dat wordt gebruikt om een nieuwe git repository te maken. Het kan worden gebruikt om een bestaand, niet-versiebeheerd project om te zetten naar een git repository of om een nieuwe, lege repository te initialiseren. De meeste andere git commando’s zijn niet beschikbaar buiten een geïnitialiseerde repository, dus dit is meestal het eerste commando dat je uitvoert in een nieuw project.

De algemene syntax voor het commando is: `git init [<opties>] [<directory>]`. Enkele veelgebruikte opties zijn:

- `-bare` om een kale repository te maken. Dit is een repository die alleen wordt gebruikt als een externe repository en die geen actieve ontwikkeling bevat. De repository bevat alleen de .git directory en geen werkdirectory.
- `-template=<template-directory>` om een directory op te geven waaruit sjablonen worden gebruikt. Dit zijn bestanden die worden gekopieerd naar de .git directory van de nieuwe repository, zoals hooks of beschrijvingen.
- `-separate-git-dir=<git-dir>` om een tekstbestand te maken in plaats van een directory voor de .git directory van de repository. Dit bestand bevat het pad naar de eigenlijke repository en fungeert als een bestandssysteemonafhankelijke git symbolische link naar de repository.
- `-b <branch-name>` of `--initial-branch=<branch-name>` om de naam op te geven voor de initiële branch in de nieuwe repository. Als dit niet wordt opgegeven, wordt de standaardnaam gebruikt (momenteel master, maar dit kan in de toekomst veranderen; de naam kan worden aangepast via de init.defaultBranch configuratievariabele).
- `-shared [= (false|true|umask|group|all|world|everybody|<perm>)]` om aan te geven dat de git repository moet worden gedeeld tussen meerdere gebruikers. Dit maakt het mogelijk dat gebruikers die tot dezelfde groep behoren naar die repository kunnen pushen. Wanneer dit wordt opgegeven, wordt de configuratievariabele “core.sharedRepository” ingesteld zodat bestanden en directories onder $GIT_DIR worden aangemaakt met de gevraagde permissies.

## Usefull links

- [OpenCommit](https://github.com/marketplace/actions/opencommit-improve-commits-with-ai): Auto-generate meaningful commits in 1 second
- [Gitmoji](https://github.com/carloscuesta/gitmoji): Gitmoji is an initiative to standardize and explain the use of emojis on GitHub commit messages.