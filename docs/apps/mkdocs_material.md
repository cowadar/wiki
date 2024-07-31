# Getting started

Url: [https://pypi.org/project/mkdocs-material/](https://pypi.org/project/mkdocs-material/)

Material for Mkdocs is een thema voor [Mkdocs](./mkdocs.md), een statische site generator die gericht is op (technische) projectdocumentatie. Met Material for Mkdocs kun je je documentatie schrijven in Markdown en een professionele statische site maken in enkele minuten - doorzoekbaar, aanpasbaar, voor alle apparaten. Je kunt Material for Mkdocs installeren met pip, de Python package manager, of met docker.


## Installatie

### Met `pip`

Material for MkDocs wordt gepubliceerd als een [Python package]() en kan worden geïnstalleerd met `pip`, idealiter door gebruik te maken van een [virtual environment]().
Open een terminal en installeer Material for MkDocs met:

```bash
pip install mkdocs-material
```

!!! note
    Dit zal automatisch compatibele versies van alle afhankelijkheden installeren: [MkDocs], [Markdown], [Pygments] en [Python Markdown Extensions]. Material for MkDocs streeft er altijd naar om de nieuwste versies te ondersteunen, dus het is niet nodig om die pakketten apart te installeren.


### Met docker

De officiële [Docker image] is een geweldige manier om snel aan de slag te gaan, want het bevat alle afhankelijkheden die vooraf geïnstalleerd zijn. Open een terminal en haal de image op met:

```bash
docker pull squidfunk/mkdocs-material
```

Het `mkdocs` uitvoerbaar bestand wordt aangeboden als een entry point en `serve` is het standaard commando.

??? question "How to add plugins to the Docker image?"

    Material for MkDocs bundelt alleen geselecteerde plugins om de grootte
    van de officiële image klein te houden. Als de plugin die je wilt gebruiken niet is inbegrepen,
    maak dan een nieuw `Dockerfile` aan en breid de officiële Docker image uit:

    ```Docker
    FROM squidfunk/mkdocs-material
    RUN pip install ...
    ```

    Vervolgens kun je de image bouwen met het volgende commando:

    ```
    docker build -t squidfunk/mkdocs-material .
    ```

    De nieuwe image kan precies zo worden gebruikt als de officiële image.

### met git

Material for MkDocs kan direct gebruikt worden vanaf [GitHub] door de
repository te klonen naar een submap van je project root. Dit kan handig zijn als je
de nieuwste versie wilt gebruiken:

```bash
git clone https://github.com/squidfunk/mkdocs-material.git
```

Het thema bevindt zich in de map `mkdocs-material/material`. Na het klonen
van `git`, moet je alle vereiste afhankelijkheden installeren met:

```bash
pip install -e mkdocs-material
```

  [GitHub]: https://github.com/squidfunk/mkdocs-material
