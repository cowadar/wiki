# DDNS
Een DDNS is noodzakelijk als je met een Dynamisch WAN ip zit.
Deze cheked om de zoveel tijd of je WAN IP veranderd is en past dit dan automatisch aan in Cloudflare.

??? "docker-compose.yaml"

    ```yaml
    services:
    cloudflare-ddns:
        image: oznu/cloudflare-ddns:latest
        restart: always
        environment:
        - API_KEY=xxxxxxx
        - ZONE=example.com
        - SUBDOMAIN=subdomain #optional
        - PROXIED=false #its better to use true
    ```

## Creating a Cloudflare API token
To create a CloudFlare API token for your DNS zone go to https://dash.cloudflare.com/profile/api-tokens⁠ and follow these steps:

1. Click Create Token
2. Provide the token a name, for example, cloudflare-ddns
3. Grant the token the following permissions:
    * Zone - Zone Settings - Read
    * Zone - Zone - Read
    * Zone - DNS - Edit
4. Set the zone resources to:
    * Include - All zones
5. Complete the wizard and copy the generated token into the API_KEY variable for the container