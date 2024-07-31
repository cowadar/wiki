# Cloudflare Tunnel

## Protect your web servers from direct attack

From the moment an application is deployed, developers and IT spend time locking it down — configuring ACLs, rotating IP addresses, and using clunky solutions like GRE tunnels.

There’s a simpler and more secure way to protect your applications and web servers from direct attacks: Cloudflare Tunnel.

Ensure your server is safe, no matter where it’s running: public cloud, private cloud, Kubernetes cluster, or even a Mac mini under your TV.

## I do everthing in the cli

install the cloudflare tunnel service.
in my case i will do the install on een ubuntu machine.

```bash
wget -q https://github.com/cloudflare/cloudflared/releases/latest/download/cloudflared-linux-amd64.deb && sudo dpkg -i cloudflared-linux-amd64.deb
```

When you run the flowing command you get a url. login to cloudflare

```bash
cloudflared tunnel login
```

!!! tip
    When cloudflare is connected you get a cert.pem.
    Make a note of the location.

create the tunnel
by name fill the name that you want for the tunnel.

```bash
cloudflared tunnel create <NAME>
```

!!! tip
    Take a note where your tunnel credentials are saved.

create a configuration file in the `.cloudflared` directory

```bash
nano /home/$USER/.cloudflared/config.yaml
```

Set the following lines.

```bash
tunnel: Your-Tunnel-Id
credentials-file: /home/$USER/.cloudflared/1d4537b6-67b9-4c75-a022-ce805acd5c0a.json

1d4537b6-67b9-4c75-a022-ce805acd5c0a.json # Get the json file from previous step.
```

add your first site example.com

```bash
cloudflared tunnel route dns <name of the tunnel> <example.com>
```

create the ingress.
create config.yml file in you .cloudflared directory

```bash
ingress:
  - hostname: example.com
    service: http://internalip:80
  - hostname: sub.example.com
    service: http://internalip:88
  - service: http_status:404 # this is required as a 'catch-all'
```

start the tunnel

```bash
cloudflared tunnel run <name of your tunnel>
```

Make a service to run automatic

```bash
cloudflared service install
```

start en enable the service

```bash
systemctl enable --now cloudflared
```
