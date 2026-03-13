---
title: "Configuring Mautrix Discord"
date: 2026-03-13 12:00:00 +/-TTTT
categories: [matrix]
tags: [technical]    
description: 
toc: true
---

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

## Setting up NGINX for HTTPS on the VM
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

## Generating the cert

Run the following command to generate the cert
```bash
certbot --nginx -d example.org
```
If a prompt shows up, select `2`

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

## Creating the folders for it
I will be using `/matrix2` in this case:

```bash
mkdir -p /matrix2/synapse /matrix2/mautrix-discord-bot
```

## Create docker-compose.yml

```bash
cd /matrix2/synapse
```

```bash
services:
  synapse:
    image: matrixdotorg/synapse:latest
    restart: unless-stopped
    ports:
      - "8008:8008"
    volumes:
      - ./synapse-data:/data
    environment:
      - SYNAPSE_SERVER_NAME=example.org
      - SYNAPSE_REPORT_STATS=no

  mautrix-discord-relay:
    image: dock.mau.dev/mautrix/discord:latest
    restart: unless-stopped
    volumes:
      - /matrix2/mautrix-discord-relay:/data
    depends_on:
      - mautrix-discord-relay-postgres

  postgres:
    image: postgres:16-alpine
    restart: unless-stopped
    environment:
      POSTGRES_PASSWORD: synapse_postgres
      POSTGRES_USER: synapse
      POSTGRES_DB: synapse
      POSTGRES_INITDB_ARGS: "--encoding=UTF-8 --lc-collate=C --lc-ctype=C"
    volumes:
      - ./postgres-data:/var/lib/postgresql/data

  mautrix-discord-relay-postgres:
    image: postgres:16-alpine
    restart: unless-stopped
    environment:
      POSTGRES_USER: mautrix_D_relay_user
      POSTGRES_PASSWORD: mautrix_D_relay_password
      POSTGRES_DB: mautrix_discord_relay
    volumes:
      - ./mautrix-discord-relay-postgres-data:/var/lib/postgresql/data
```

## Create homeserver.yaml

```bash
mkdir -p /matrix2/synapse/synapse-data
```
```bash
nano /matrix2/synapse/synapse-data/homeserver.yaml 
```
```bash
server_name: "exampple.org"
public_baseurl: "https://example.org/"

listeners:
  - port: 8008
    tls: false
    type: http
    x_forwarded: true
    bind_addresses: ['::', '0.0.0.0']
    resources:
      - names: [client, federation]
        compress: false

database:
  name: psycopg2
  args:
    user: synapse
    password: synapse_postgres
    database: synapse
    host: postgres
    cp_min: 5
    cp_max: 10

app_service_config_files:
  - /data/mautrix-discord-registration.yaml

media_store_path: /data/media_store
report_stats: false
registration_requires_token: false               # or set a shared secret if you want controlled signups
registration_shared_secret: ""
report_stats: false
```

## Create registration.yaml

```bash
cd /matrix2/mautrix-discord-bot
```

```bash
nano registration.yaml
```

```bash
id: discord
url: http://mautrix-discord-relay:29334
as_token: 
hs_token: 
sender_localpart: 
rate_limited: false
namespaces:
    users:
        - regex: ^@discordrelay:example\.org$
          exclusive: true
        - regex: ^@discordpuppet_.*:example\.org$
          exclusive: true
de.sorunome.msc2409.push_ephemeral: true
push_ephemeral: true
```
Edit the values of `users`. These are the names your bot and 'relayed' users will have.

## Generating new as_token and hs_token:

```bash
cd /matrix2/synapse
```
```bash
docker compose run --rm mautrix-discord-relay /usr/bin/mautrix-discord -g -c /matrix2/mautrix-discord-relay/config.yaml -r /matrix2/mautrix-discord-relay/registration.yaml
```

## Editing new config.yaml

Homeserver:
```bash
# Homeserver details.
homeserver:
    # The address that this appservice can use to connect to the homeserver.
    address: https://example.org
    # The domain of the homeserver (also known as server_name, used for MXIDs, etc).
    domain: example.org
```

Database:
```bash
    database:
        # The database type. "sqlite3-fk-wal" and "postgres" are supported.
        type: postgres
        # The database URI.
        #   SQLite: A raw file path is supported, but `file:<path>?_txlock=immediate` is recommended.
        #           https://github.com/mattn/go-sqlite3#connection-string
        #   Postgres: Connection string. For example, postgres://user:password@host/database?sslmode=disable
        #             To connect via Unix socket, use something like postgres:///dbname?host=/var/run/postgresql
        uri: "postgres://mautrix_D_relay:mautrix_D_relay_password@mautrix-discord-relay-postgres:5432/mautrix_discord_relay?sslmode=disable"
        # Maximum number of connections. Mostly relevant for Postgres.
        max_open_conns: 20
        max_idle_conns: 2
        # Maximum connection idle time and lifetime before they're closed. Disabled if null.
        # Parsed with https://pkg.go.dev/time#ParseDuration
        max_conn_idle_time: null
        max_conn_lifetime: null
```

Name of your bot. This should match with `registration.yaml`'s first user that is exclusive

`Config.yaml`:

```bash
# The unique ID of this appservice.
    id: discord
    # Appservice bot details.
    bot:
        # Username of the appservice bot.
        username: discordrelay
        # Display name and avatar for bot. Set to "remove" to remove display name/avatar, leave empty
        # to leave display name/avatar as-is.
        displayname: discordrelay
        avatar: mxc://maunium.net/nIdEykemnwdisvHbpxflpDlC

```

`Registration.yaml`:

```bash
namespaces:
    users:
        - regex: ^@discordrelay:example\.org$
          exclusive: true
```

Name of puppet users. This should match with `registration.yaml`'s second user that is non exclusive

`Config.yaml`:

```bash
# Bridge config
bridge:
    # Localpart template of MXIDs for Discord users.
    # {{.}} is replaced with the internal ID of the Discord user.
    username_template: discordpuppet_{{.}}
```

`Registration.yaml`:
```bash
        - regex: ^@discordpuppet_.*:example\.org$
          exclusive: true
```

## Copy registration.yaml over to synapse-data 

```bash
cp /matrix2/mautrix-discord-relay/registration.yaml /matrix2/synapse/synapse-data/mautrix-discord-registration.yaml
```

## Setting permissions

```bash
chown -R 991:991 /matrix2/synapse
```

## Starting it
Now start it up:

```bash
cd /matrix2/synapse
docker compose up
```

## Creating your admin user

```bash
docker compose exec synapse register_new_matrix_user --admin -c /data/homeserver.yaml http://localhost:8008
```

## Edit permissions in config.yaml

```bash
    permissions:
        "*": relay
        "": user
        "@YOUR_ADMIN_USER:example.org": admin

```

## Restart the containers

```bash
docker compose down
docker compose up
```

## Log in as your admin user
Now go to <https://app.element.io/>, select the domain name of the homeserver you're using. Afterwards enter the credentials of the admin user you set a while ago.

## Create a room
Create a matrix room and invite your bot.


