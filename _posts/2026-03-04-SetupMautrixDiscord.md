---
title: "Setting up Mautrix Discord"
date: 2026-03-03 12:00:00 +/-TTTT
categories: [General]
tags: [Flipper]    
description: 
toc: true
---
## Introduction
This guide will cover up how I self host mautrix-discord (<https://docs.mau.fi/bridges/go/discord/authentication.html>). 

## Prerequisites
Firstly, you will need a public facing domain name. Refer to my previous article on obtaining a domain name for free. Secondly, it is best if this is done on a cloud virtual machine. In this case, i am using an Ubuntu VM from DigitalOcean. (not sponsored btw). These are the specs i am running on the VM:

![screenshot of droplet specs](assets/posts/post4/post4_img4_dropletstats.png)


## Setting up the VM
These are the commands to run to set up your VM. 

Updating the system:

```bash
apt update && apt upgrade -y
```

Installing needed tools:

``` bash
apt install -y nano wget curl git openssl net-tools ufw
```

Setting up the firewall (UFW):

```bash
ufw allow OpenSSH
ufw allow 80/tcp
ufw allow 443/tcp
ufw allow 8448/tcp   
ufw enable
```

## Installing docker on the VM
We will be using docker to run our synapse server on. Refer here (<https://docs.docker.com/engine/install/>) for instructions to install docker for other distros. These are the commands i used as i was on Ubuntu (<https://docs.docker.com/engine/install/ubuntu/>)

Installing docker:

```bash
# Add Docker's official GPG key:
sudo apt update
sudo apt install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
sudo tee /etc/apt/sources.list.d/docker.sources <<EOF
Types: deb
URIs: https://download.docker.com/linux/ubuntu
Suites: $(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}")
Components: stable
Signed-By: /etc/apt/keyrings/docker.asc
EOF

sudo apt update
```

## Setting up NGINX + CertBot for HTTPS on the VM
HTTPS is needed to allow the bridge to work, hence we will set it up.

Installing NGINX and CertBot:

```bash
apt install -y snapd
snap install core; snap refresh core
snap install --classic certbot
ln -s /snap/bin/certbot /usr/bin/certbot
apt install -y nginx
systemctl enable --now nginx
```

Creating the NGINX site config:

``` bash
mkdir -p /etc/nginx/sites-available /etc/nginx/sites-enabled
nano /etc/nginx/sites-available/matrix
```

``` bash
server {
    listen 80;
    server_name example.org;

    return 301 https://$host$request_uri;  # Only here – good
}

server {
    listen 443 ssl http2;
    server_name example.org;

    ssl_certificate /etc/letsencrypt/live/example.org/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/example.org/privkey.pem;

    location / {
        proxy_read_timeout 360s;
        proxy_send_timeout 360s;
        proxy_connect_timeout 360s;
        proxy_buffers 8 64k;
        proxy_buffer_size 64k;
        proxy_next_upstream error timeout invalid_header http_502 http_503 http_504;
        proxy_next_upstream_tries 3;
        proxy_pass http://127.0.0.1:8008;
        proxy_set_header X-Forwarded-For $remote_addr;
        proxy_set_header X-Forwarded-Proto $scheme;          
        proxy_set_header Host $host;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
    }
}
```

Generating the cert:

```bash
certbot --nginx -d example.org
```

Enabling the site:

```bash
ln -s /etc/nginx/sites-available/matrix /etc/nginx/sites-enabled/
rm /etc/nginx/sites-enabled/default
nginx -t
systemctl reload nginx
```

Testing if HTTPS works:

```bash
curl example.org
```

## Setting up synapse server docker container on the VM
You will now set up the synapse server for the mautrix discord bridge service to run on. 

Creating the synapse directory:

```bash
mkdir -p /opt/synapse
cd /opt/synapse
```

Creating docker-compose.yml:

```bash
nano docker-compose.yml
```

```bash
version: '3.8'

services:
  postgres:
    image: postgres:16-alpine
    restart: unless-stopped
    environment:
      POSTGRES_PASSWORD: SYNAPSE
      POSTGRES_USER: synapse
      POSTGRES_DB: synapse
      POSTGRES_INITDB_ARGS: "--encoding=UTF-8 --lc-collate=C --lc-ctype=C"
    volumes:
      - ./postgres-data:/var/lib/postgresql/data

  synapse:
    image: matrixdotorg/synapse:latest
    restart: unless-stopped
    ports:
      - "8008:8008"
    volumes:
      - ./synapse-data:/data
    depends_on:
      - postgres
```

Generating the config:

```bash
docker compose run --rm -e SYNAPSE_SERVER_NAME=.example.org -e SYNAPSE_REPORT_STATS=no synapse generate
```

Editing homeserver.yaml:

```bash
nano synapse-data/homeserver.yaml
```

```bash
server_name: "example.org"
public_baseurl: "https://example.org"

database:
  name: psycopg2
  args:
    user: synapse
    password: SYNAPSE
    database: synapse
    host: postgres
    cp_min: 5
    cp_max: 10

listeners:
  - port: 8008
    tls: false
    type: http
    x_forwarded: true
    bind_addresses: ['::1', '127.0.0.1']
    resources:
      - names: [client, federation]
        compress: false

enable_registration: false  
```

Your folders should now look like this, ignore mautrix-postgres. 

Content of opt/synapse:

```bash
docker-compose.yml postgres-data  synapse-data
```

Content of opt/synapse/synapse-data:

```bash
example.org.log.config	example.org.signing.key  homeserver.yaml  media_store
```


Starting it up:

```bash
docker compose up -d
```

Go to your website, it should show this:
![screenshot of synapse server](assets/posts/post4/post4_img1_synapseup.png)


Creating your admin user:

```bash
docker compose exec synapse register_new_matrix_user --admin -c /data/homeserver.yaml http://localhost:8008
```
This will prompt you to enter the account's username and password. If no prompt shows up, your docker container most likely crashed due to lack of RAM.


## Creating a seperate Postgres container for mautrix-discord
We will now modify docker-compose.yml to include our database for mautrix-discord.

Add this to docker-compose.yml:

```bash
mautrix-postgres:
    image: postgres:16-alpine
    container_name: mautrix-postgres
    restart: unless-stopped
    environment:
      POSTGRES_PASSWORD: MAUTRIX_POSTGRES
      POSTGRES_USER: mautrix
      POSTGRES_DB: mautrix_discord
    volumes:
      - ./mautrix-postgres-data:/var/lib/postgresql/data
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U mautrix"]
      interval: 10s
      timeout: 5s
      retries: 5
```

We will be using this same POSTGRES_PASSWORD later on for configuring our mautrix-discord service.



## Set Up mautrix-discord Bridge on the VM
We will now set up the mautrix-discord container. 

Creating its directory:

```bash
mkdir -p /opt/mautrix-discord
cd /opt/mautrix-discord
```

Generating config.yaml + registration.yaml:

```bash
docker run --rm -v "$(pwd)":/data dock.mau.dev/mautrix/discord:latest
```
Content of /mautrux-discord should be:

```bash
config.yaml  logs  registration.yaml
```


These are the lines in config.yaml you need to edit for it to work on your configuration.

Address + domain name:

```bash
    address: https://example.org
    # The domain of the homeserver (also known as server_name, used for MXIDs, etc).
    domain: example.org
```  

Database:

```bash
appservice:
    # The address that the homeserver can use to connect to this appservice.
    address: http://mautrix-discord:29334
    
    # The hostname and port where this appservice should listen.
    hostname: 0.0.0.0
    port: 29334
    
    # Database config.
    database:
        # The database type. "sqlite3-fk-wal" and "postgres" are supported.
        type: postgres
        # The database URI.
        #   SQLite: A raw file path is supported, but `file:<path>?_txlock=immediate` is recommended.
        #           https://github.com/mattn/go-sqlite3#connection-string
        #   Postgres: Connection string. For example, postgres://user:password@host/database?sslmode=disable
        #             To connect via Unix socket, use something like postgres:///dbname?host=/var/run/postgresql
        uri: "postgres://mautrix:MAUTRIX_POSTGRES@mautrix-postgres:5432/mautrix_discord?sslmode=disable"
        # Maximum number of connections. Mostly relevant for Postgres.
        max_open_conns: 20
        max_idle_conns: 2
        # Maximum connection idle time and lifetime before they're closed. Disabled if null.
        # Parsed with https://pkg.go.dev/time#ParseDuration
        max_conn_idle_time: null
        max_conn_lifetime: null
```

Adding the admin account we created earlier to permissions:

```bash
permissions:
        "*": relay
        "": user
        "@admin:example.org": admin
```

registration.yaml should look like this:

```bash
id: discord
url: http://localhost:29334
as_token: 
hs_token: 
sender_localpart: 
rate_limited: false
namespaces:
    users:
        - regex: ^@discordbot:exampe\.org$
          exclusive: true
        - regex: ^@discord_.*:example\.org$
          exclusive: true
de.sorunome.msc2409.push_ephemeral: true
push_ephemeral: true
```
as_token and hs_token should be generated via this command:

```bash
openssl rand -hex 32   
```

Copy registration file:

```bash
cp registration.yaml /opt/synapse/synapse-data/mautrix-discord-registration.yaml
```

Update homeserver.yaml:

```bash
nano /opt/synapse/synapse-data/homeserver.yaml;
```

```bash
app_service_config_files:
  - /data/mautrix-discord-registration.yaml
```

Adding the bridge to docker-compose.yml

```bash
nano /opt/synapse/docker-compose.yml
```

```bash
mautrix-discord:
  image: dock.mau.dev/mautrix/discord:latest
  container_name: mautrix-discord
  restart: unless-stopped
  volumes:
    - /opt/mautrix-discord:/data
  depends_on:
    - synapse
```

Ensuring permissions are correct:

```bash
chown -R 1000:1000 /opt/mautrix-discord
chmod 644 /opt/mautrix-discord/*.yaml
chown 1000:1000 /opt/synapse/synapse-data/mautrix-discord-registration.yaml
chmod 644 /opt/synapse/synapse-data/mautrix-discord-registration.yaml
```

Restart docker containers:

```bash
docker compose down
docker compose up -d
```
Verify it is working:

```bash
docker compose ps
docker compose logs -f mautrix-discord
```

## Inviting the bot to your matrix room
Firstly, create a matrix room, click on 'create a new conversion' > 'New room' > turn off end-to-end encryption . 
![screenshot of create convo button](assets/posts/post4/post4_img10_createnewconvo.png)

![screenshot of new room](assets/posts/post4/post4_img11_newroom.png)

![screenshot of disable e2ee](assets/posts/post4/post4_img12_disable.png)

Secondly, click on room info on the top right > invite > enter the name of your bot (discordbot:example.org). It will then proceed to join your matrix room.

![screenshot of room info button](assets/posts/post4/post4_img13_roominfo.png)

![screenshot of invite button](assets/posts/post4/post4_img14_invite.png)

## Conclusion
This sums up bridging matrix to discord and vice versa. A guide on using this will be up soon.