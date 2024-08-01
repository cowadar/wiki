# Git cheatsheet

## Git config

### Gebruiker definiëren

#### Methode 1
```bash
git config --global user.email "email@mail.com"
git config --global user.name "username"
```
#### Methode 2

Global config aanpassen via

```bash
git config --global --edit
```

Dan ziet je config er ongeveer zo uit:
```bash
$ cat ~/.gitconfig
[user]
	name = Bedar Idem
	email = <user>@<domain>
```

### File permissions problemen oplossen

```bash
git config core.filemode false
```

## Git pull

### Git pull in subfolders

Indien je in meerdere onderliggende folders git repo's hebt zitten, kan je met dit commando alle git repos laten pullen.

Dankzij [parallel](../../linux/parallel.md) zal dit niet tegelijkertijd uitgevoerd worden, maar op meerdere tasks in parallel.

```bash
find . -maxdepth 8 -name '.git' -prune -type d -printf '%h\n' | parallel --eta 'echo {} && git -C {} pull'
```

## Git alias

Als je dit in je terminal ingeeft, kan je commando's sneller schrijven met korte **aliassen**.

```bash
function gc () {
 git commit -m "$*"
}

alias ga='git add'
alias gp='git push'
alias gl='git log'
alias gs='git status'
alias gd='git diff'
alias gdc='git diff --cached'
alias gb='git branch'
alias gra='git remote add'
alias grr='git remote rm'
alias gpu='git pull'
alias gcl='git clone'
```

## Git Fetch

Git fetch is een commando dat commits, bestanden, branches en tags ophaalt van een externe repository naar je lokale repository. De algemene syntax voor het commando is: `git fetch <opties> <externe naam> <branch naam>` Git isoleert de opgehaalde inhoud van de lokale code. Daarom biedt de fetch een veilige manier om de informatie te bekijken voordat je deze commit naar je lokale branch

### Fetch alle externe branches

```bash
git fetch --all
```

### Fetch enkel 1 branch

```bash
git fetch origin <local-branch-name>:<remote-branch-name>
```

## Git branch

Git branch is een commando dat branches lijst, maakt, hernoemt of verwijdert. Een branch is een onafhankelijke lijn van ontwikkeling. Branches dienen als een abstractie voor het bewerken/stagen/commiten proces. Je kunt ze beschouwen als een manier om een nieuwe werkmap, staging area en projectgeschiedenis aan te vragen. Nieuwe commits worden vastgelegd in de geschiedenis van de huidige branch, wat resulteert in een splitsing in de geschiedenis van het project.

De algemene syntax voor het commando is: `git branch <opties> <branch naam>`. Enkele veelgebruikte opties zijn:

- `-d` om een branch te verwijderen. Dit is een “veilige” operatie omdat Git je verhindert om de branch te verwijderen als deze niet-samengevoegde wijzigingen heeft.
- `-D` om een branch geforceerd te verwijderen, zelfs als deze niet-samengevoegde wijzigingen heeft. Dit is het commando om te gebruiken als je alle commits die verband houden met een bepaalde ontwikkelingslijn permanent wilt weggooien.
- `-m` om de huidige branch te hernoemen.
- `-a` om zowel lokale als externe branches te tonen.
Het git branch commando laat je niet toe om tussen branches te wisselen of een gesplitste geschiedenis weer samen te voegen. Daarom is git branch nauw geïntegreerd met de git checkout en git merge commando’s

## Branch verwijderen

```bash
git branch -D branch-name
```

### Laat alle locale branches zien

```bash
git branch -a
```