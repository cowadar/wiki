# Ansible
## Installeer Ansible
Voor je begint zijn er een aandere stappen die je moet doorlopen.
Waaronder het installeren van Ansible
```bash
sudo add-apt-repository --yes --update ppa:ansible/ansible
sudo apt install ansible -y
ansible --version
```

### Voor de het target toestel installeer je python
Als python aan de andere kant niet is geinstalleerd zal ansible ook niet werken.
```bash
sudo apt install python3
sudo apt install python3-pip
```

### Test de connectie
Voor we verder gaan willen we weten of de connectie wel werkt.
Dat doe je door volgende commando uit te voeren.
!!! note
sshpass is nodig voor directe ansible commandos

```bash
sudo apt-get install sshpass
ansible all -i "IP," -m ping -u USER -k
```

## installeer ssh keys
Je kan ansible laten werken zonder ssh keys. Met ssh keys is die gemakkelijker en gebruiks vriendelijker.

### Installeer een ssh key voor ansible
Dit doen we om een ssh key voor ansible te reserveren.
```bash
ssh-keygen -t ed25519 -C "jouw_email@voorbeeld.com"
```
Zet zeker volgende path.
"~/.ssh/id_ansible"
```bash 
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ansible
cat ~/.ssh/id_ansible.pub
```
!!! note
Als je in verdere stappen problemen krijgt met het remote connectie maken voor dan volgende commandie uit
```bash
ssh-copy-id -i ~/.ssh/id_ansible.pub username@remote-host
```

### Github ssh key
Als je met github werkt raad ik aan een key aan te maken en de public in github te steken
Deze key word ook meegegeven aan alle servers waar je ansible op uitvoert.

```bash
ssh-keygen -t ed25519 -C "jouw_email@voorbeeld.com"
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
cat ~/.ssh/id_ed25519.pub
```

#### Github test
Test even of de connectie met Github werkt voor je verdergaat.
Als dit failt zullen alle toestellen waar je deze key opzet ook failen.
```bash
ssh -T git@github.com
```

## test script
Eerst en vooral zul je dit commano moeten losslaten omdat je nog geen connectie hebt met de nodes.
Dit Commandoe vraagt achter het gebruikers wachtwoord en het root wachwoord
```bash
ansible-playbook  test_playbook.yml -u USER -k -K
```
Als Voorig scipt succesvol is uitgevoerd zou je vanaf nu je script kunnen starten met
```bash
ansible-playbook  test_playbook.yml
```
