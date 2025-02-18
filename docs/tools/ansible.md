# Ansible
## Wat is Ansible
Ansible is een open-source automatiseringstool waarmee je IT-taken zoals configuratiebeheer, software-implementatie en applicatie-orkestratie eenvoudig kunt uitvoeren. Het werkt agentloos, wat betekent dat je geen extra software hoeft te installeren op de beheerde systemen. Ansible gebruikt YAML-bestanden (playbooks) om instructies te definiëren en maakt verbinding via SSH of WinRM om taken uit te voeren. Het is populair vanwege de eenvoudige syntaxis, schaalbaarheid en efficiëntie in het beheren van zowel kleine als grote IT-omgevingen.

## Benodigdheden voor het uitvoeren van een Ansible-script (easy)

Om een Ansible-script te laten draaien, zijn er een aantal essentiële onderdelen:

```bash
ansible
  ├── inventory.md
  └── playbook.yml
```
### Inventory-bestand

Het inventory-bestand bevat een lijst van servers waarop Ansible taken moet uitvoeren. Een eenvoudig voorbeeld:

```bash [webservers]
server1.example.com
server2.example.com

[databases]
db1.example.com
```
Je kunt ook variabelen specificeren per groep of per host.

### Playbook-bestand

Een playbook is een YAML-bestand waarin je de taken definieert die Ansible moet uitvoeren. Een voorbeeld van een eenvoudig playbook:

```bash
---
- name: Installeer Apache
  hosts: webservers
  become: yes
  tasks:
    - name: Update pakkettenlijst
      apt:
        update_cache: yes
    - name: Installeer Apache
      apt:
        name: apache2
        state: present
```
Ansible uitvoeren

Als het inventory- en playbook-bestand klaar zijn, kun je het script uitvoeren met:

```bash 
ansible-playbook -i inventory playbook.yml
```

## Benodigdheden voor het uitvoeren van een Ansible-script (advanced)
Als je heb bovenste onder de knie hebt kan je ook dit proberen voor een groter Ansible script.
```bash
ansible
   ├── ansible.cfg
   ├── files
   │   └── sudoers_USER
   ├── inventory.md
   ├── roles
   │   └── playbook1
   │       └── tasks
   │           └── main.yml
   └── test_playbook.yml
```
### Uitleg van de bestanden en mappen:

- files/: Bevat bestanden die nodig zijn voor de configuratie of implementatie.
- roles/: Hierin kunnen Ansible-rollen worden opgeslagen, die taken groeperen per functie.
- ansible.cfg: De configuratie van Ansible, waarin instellingen zoals de locatie van het inventory-bestand en logbestanden staan.
- inventory.md: Bevat een lijst van de servers en hun groepen die door Ansible worden beheerd.
- test_playbook.yml: Een test-playbook om de basisfunctionaliteit van Ansible te controleren.

??? "Configuratie bestanden"
    !!! warning
        Dit is een voorbeeld!!
    === "ansible.cfg"

        ```yaml
            [defaults]
            interpreter_python=auto_silent
            host_key_checking=false
        ```
    === "sudoers_USER"
        
        ```yaml
            cowarol ALL=(ALL) NOPASSWD: ALL
        ```
    === "inventory.md"

        ```yaml
            [docker]
            10.18.0.20 #network
            10.18.0.21 #Media
            10.18.0.25 #test docker

            [docker:vars]
            ansible_connection=ssh  
            ansible_user=USER   
            ansible_ssh_private_key_file=~/.ssh/id_ansible
        ```
    === "main.yaml"

        ```yaml
            ---
            - name: install sudo package
            become: true
            apt:
                name: sudo 
                update_cache: yes
                cache_valid_time: 3600
                state: latest

            - name: create USER user
            user: 
                name: USER
                shell: '/bin/bash'

            - name: add USER to sudoers
            become: true
            copy:
                src: sudoers_USER
                dest: /etc/sudoers.d/USER
                owner: root
                group: root
                mode: 0440

            - name: Copy SSH public key to remote host
            ansible.builtin.authorized_key:
                user: cowarol
                key: "{{ lookup('file', '~/.ssh/id_ansible.pub') }}"
                state: present

            - name: Copy SSH keys
            become: true
            ansible.builtin.copy:
                src: "/home/USER/.ssh/{{ item.file }}"
                dest: "/home/USER/.ssh/{{ item.file }}"
                owner: cowarol
                group: cowarol
                mode: "{{ item.mode }}"
            loop:
                - { file: 'id_ed25519', mode: '0600' }
                - { file: 'id_ed25519.pub', mode: '0644' }


            - name: Ensure SSH agent is running
            ansible.builtin.shell: eval $(ssh-agent)
            changed_when: false

            - name: Test GitHub SSH connectivity
            ansible.builtin.command: ssh -T git@github.com
            register: ssh_test
            failed_when: 
                - ssh_test.rc != 1
                - '"successfully authenticated" not in ssh_test.stderr'
            changed_when: false
        ```
    === "test_playbook.yaml"
        ```yaml
            ---
            - name: docker Playbook
            hosts: docker
            gather_facts: true
            become: false
            roles:
                - install_ssh-ansible

        ```

    === "readme.md"
        # Ansible
        > [!NOTE]  
        > Voor meer informatie https://wiki.cowadar.be/tools/ansible/

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
        > [!NOTE]  
        > sshpass is nodig voor directe ansible commando

        ```bash
        sudo apt-get install sshpass
        ansible all -i "IP," -m ping -u USER -k
        ```

        ## installeer ssh keys
        Je kan ansible laten werken zonder ssh keys. Met ssh keys is dit gemakkelijker en gebruiks vriendelijker.

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
        > [!NOTE]  
        > Als je in verdere stappen problemen krijgt met het remote connectie maken voer dan volgende commandie uit

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
        > [!warning]  
        > Voor je verdergaat ga je naar de inventory.md file en pas je jouw waardes aan.
        > De test_playbook spreekt de [docker] aan.
        ```bash
        ansible-playbook  test_playbook.yml -u USER -k -K
        ```
        Als Voorig scipt succesvol is uitgevoerd zou je vanaf nu je script kunnen starten met
        ```bash
        ansible-playbook  test_playbook.yml
        ```