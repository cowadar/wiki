# Mariadb


## Installeer MariaDB op Ubuntu 20.04 LTS
```bash
sudo apt update
sudo apt install mariadb-server
sudo mysql_secure_installation
```

## Admin user aanmaken
1. Maak een nieuwe gebruiker `newuser` aan voor de host `localhost` met een nieuw `password`:

```mysql
CREATE USER 'newuser'@'localhost' IDENTIFIED BY 'password';
```

2. Geef alle rechten aan de nieuwe gebruiker
```mysql
GRANT ALL PRIVILEGES ON * . * TO 'newuser'@'localhost';
```

3. Werk de rechten bij
```mysql
FLUSH PRIVILEGES;
```