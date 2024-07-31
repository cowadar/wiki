# Cowadar - PKM

Onze mkdocs website bestaat uit 2 repositories:

- cowadar/cowadar.github.io
- cowadar/pkm

## Repository clonen naar eigen computer

!!! note "Requirements"
    `git`, `python` & `mkdocs` moeten geinstalleerd zijn

Clone de hoofd repository naar je computer

```bash
git clone git@github.com:cowadar/cowadar.github.io.git
```

Navigeer naar de folder

```bash
cd cowadar.github.io
```

Gebruik submodule om de pkm submodule binnen te halen

```bash
git submodule update --init --recursive
```

!!! note
    Dit commando moet in de root folder uitgevoerd worden.

Installeer de requirements

```bash
pip install -r requirements.txt
```

## Aanpassingen maken en pushen

Aanpassingen maken is dezelfde procedure als gewoonlijk.
Echter als er iets in de `pkm` aangepast wordt, zal de hoofdrepo ook changes laten zien. Deze moeten uiteraard ook gepusht worden

## Website beschikbaar maken online

Momenteel is de workflow nog niet 100% op punt en zal je manueel volgend commando moeten uitvoeren om de website online te krijgen.

```bash
mkdocs gh-deploy
```
