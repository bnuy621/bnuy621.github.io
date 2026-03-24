---
title: Hosting a minecraft server on a cloud VM
date: 2026-03-24 00:00:00 +0800
categories: [misc]
tags: [non technical]     # TAG names should always be lowercase
description: guide to hosting a modded minecraft server on a cloud computer
toc: true
---

## Introduction
This is to guide others how to set up a modded minecraft server on a cloud computer. In this case i will be using DigitalOcean's droplet.


## Prerequisites
Set up a public and private key for your non-root user. 


## Steps

### Step 1 - SSH into the cloud computer
Ensure that your cloud computer is configured with your system's root user public key as we will first SSH into it using our local root account. 

### Step 2 - Updating the current packages
After you SSH into the cloud computer, now update the current packages. In this case my cloud computer is running ubuntu:

```bash
sudo apt update && sudo apt upgrade -y
```
Afterwards, reboot the system.

### Step 3 - Install java
Use the following command to install java:

```bash
sudo apt install openjdk-17-jre-headless screen wget unzip -y
```


### Step 4 - Creating a user
Now create a user. This user should be the same as your system's current user. For example if your local system's user is `bnuy`, create a user on the cloud computer called `bnuy` too. This is to ensure you can `SFTP` into the cloud computer using your current user.

Create user:
```bash
useradd -m bnuy
```

### Step 5 - Ensure user has public keys
Ensure your user on the cloud computer shares the same public key with your local user:


```bash
cat /home/raven621/.ssh/authorized_keys
```

If it doesnt, paste the public key inside it:

``` bash
nano /home/raven621/.ssh/authorized_keys
```

### Step 6 - Creating folder for the modpack
Afterwards, exit the SSH session as root and now SSH into it as the new user. Once successful, create a new directory for the modpack. In this case i am using `craft to exile 2` so i will call it `cte2`:

```bash
mkdir cte2
```

### Step 7 - Setting up filezilla for SFTP
Install filezilla on your system, afterwards add your current user's `private key` to it (Edit > Settings > SFTP ):

![add private key](assets/posts/post28/post28_img1_SFTPadd.png)

Next try connecting to your cloud VM, make sure you fill in the details correctly. `host` is the IP address of the cloud VM, `User` will be the username of the user you created earlier on, `password` is the passphrase for your private key, `port` will be `22`. 

![sftp details](assets/posts/post28/post28_img2_SFTPdetails.png)

If successful, the user's home directory will be displayed:

![sftp home directory](assets/posts/post28/post28_img3_SFTPwork.png)

### Step 8 - adding the serverpack
Open the folder you created earlier on to store the modpack:

![cte2 folder](assets/posts/post28/post28_img4_cte2folder.png)

Now drag the modpack `.zip` file over to it. Afterwards, extract the content.

### Step 9 - Allow port 25565
Allow port `25565` so you can connect to your server:

```bash
ufw allow 25565/tcp
```

### Step 10 - Customising it
Now you can customise the server to your liking, such as changing the icon, `server.properties`, etc. 


### Step 11 - running it
To ensure your server doesnt stop when you exit the SSH session, run the following command:

```bash
screen -S main
```
Your terminal will clear. 

Allow `run.sh` to be executed:

```bash
chmod +X run.sh
```

Execute `run.sh`:

```bash
./run.sh
```

![cte2 folder](assets/posts/post28/post28_img5_runstart.png)

Once it is done booting up, detach from the `main` space via pressing `CTRL A` followed `D`. The terminal will show `[detached from XXXX main]`. Now exit the SSH session.

## Results

## Conclusion


