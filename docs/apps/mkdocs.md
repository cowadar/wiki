# Mkdocs

## Vereisten

```bash
sudo apt install python3-pipis
```

## Installatie

1. Installeer mkdocs (liefst in WSL of debian-based distro)

	```bash
	pip install mkdocs
	```

2. Installeer de requirements

	```bash
	pip install -r requirements.txt
	```

!!! note
		Deze commando's moeten uiteraard in de root-folder van het project uitgevoerd worden.

3. Serve de website lokaal

	```bash
	mkdocs serve
	```