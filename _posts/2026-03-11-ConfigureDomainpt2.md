---
title: "Configuring a domain name"
date: 2026-03-03 12:00:00 +/-TTTT
categories: [matrix]
tags: [technical]    
description: Still a WIP
toc: true
---

## Introduction
This is similar to the mautrix discord setting up guide except it only contains the part to setup the domain name.

## Prerequisites
Firstly, you will need a public facing domain name. Refer to my previous post on obtaining a domain name for free <https://bnuy621.github.io/posts/GettingADomainName/>. Secondly, it is best if this is done on a cloud virtual machine. In this case, i am using an Ubuntu VM from DigitalOcean. (not sponsored btw). These are the specs i am running on the VM:

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
## Setting up NGINX + CertBot for HTTPS on the VM
HTTPS is needed, hence we will set it up.

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
nano /etc/nginx/sites-available/example
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
ln -s /etc/nginx/sites-available/example /etc/nginx/sites-enabled/
rm /etc/nginx/sites-enabled/default
nginx -t
systemctl reload nginx
```

Testing if HTTPS works:

```bash
curl example.org
```
