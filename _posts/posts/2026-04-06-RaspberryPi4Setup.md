---
title: Raspberry Pi 4 - CYT
date: 2026-04-06 00:00:00 +0800
categories: [misc]
tags: [non technical]     
description: 
toc: true
---

## Introduction
Setting up chase your tail on the pi4. 

## What I am using
A raspberry pi 4B running the latest OS
A tp-link Archer T2U Plus Ver1.0



## Installing the OS
As i am using arch, i will be using the command:


### Installing rpi-imager
```bash
paru -S rpi-imager
```

### Running rpi-imager

You are required to run it as `root`. However, if you try to run this command:

```bash
sudo rpi-imager
```
![](assets/posts/post32/img1.png)


You may get a permission related error. If you don't proceed to the next step. To fix it, run this while NOT on root:


```bash
xhost +local:root
```

Afterwards, run `sudo rpi-imager` and it will work. Proceed with imaging it. Take note to have the user `pi` created.



## Updating the OS
After successfully booting up rasbian, now upgrade your system:

```bash
sudo apt update && sudo apt full-upgrade -y
sudo reboot
```

(img of upgrade progress from phone)





## Configure wifi

### Why?
You might be asking why buy ANOTHER wifi adapter when it already comes with one. The answer being the built-in one does NOT support monitor mode and that is pretty much essential for running the `Chasing your tail` project.


### How
I will be following this guide <https://github.com/lwfinger/rtw88>


### 1 - Install dkms

```bash
sudo apt install dkms
```
### 2 - Clone the rtw88 GitHub repository


```bash
git clone https://github.com/lwfinger/rtw88
```

### 3 - Build, sign, and install the rtw88 driver

```bash
cd rtw88
```

```bash
sudo dkms install $PWD
```
### 4 - Install the firmware necessary for the rtw88 driver

```bash
sudo make install_fw
```
### 5 - Copy the configuration file rtw88.conf to /etc/modprobe.d/

```bash
sudo cp rtw88.conf /etc/modprobe.d/
```

Afterwards, reboot and run `lsmod | grep -i rtw`. You will see the drivers AND the usb adapter will now have a green light indicating its in use:

(img of it)

### 6 - disabling built in adapter

The pi 4 will use `wlan0` which is the built in adapter. Hence we will now disable to built in adapter

Add this to `/boot/firmware/config.txt`:

```bash
dtoverlay=disable-wifi
```


## Installing requisites for Chasing your tail

### Python
Most will have python installed. Run `python3 --version` to ensure it is above `3.6`.


### Kismet
I will be following this guide <https://www.kismetwireless.net/docs/readme/installing/linux/>. However i will paste the commands i used here for my own future reference as well as to hopefully save the annoyance of switching between multiple tabs.

### 1 - Install dependencies

```bash
sudo apt install build-essential git libwebsockets-dev pkg-config \
zlib1g-dev libnl-3-dev libnl-genl-3-dev libcap-dev libpcap-dev \
libnm-dev libdw-dev libsqlite3-dev libsensors-dev libusb-1.0-0-dev \
libubertooth-dev libbtbb-dev libmosquitto-dev librtlsdr-dev
```
### 2 - rtl433 support
Im not too sure if i need this but just in case:

```bash
sudo apt install rtl-433
```

### 3 - clone repo
```bash
git clone https://www.kismetwireless.net/git/kismet.git
```

### 4 - configure

```bash
cd kismet
./configure
```
### 5 - compile

update the version file:

```bash
make version
```

Then, compile Kismet and the Kismet tools:

```bash
make
```
You can accelerate the process by adding -j #, depending on how many CPUs you have. To automatically compile on all the available cores:

```bash
make -j$(nproc)
```
I will be using `make -j2` as i only have 4GB ram on the pi4

```bash
sudo make suidinstall
```

Reconcile config differences as recommended by output:

(img of output show it from phone)

```bash
sudo make forceconfigs
```

Setting up the group

```bash
sudo usermod -aG kismet your-user-here
```

Now reboot.

To get it to work , click on menu (on the top right) > `data sources` > select your data source > `enable source` and you will now see information overflowing your screen. Now enjoy!

Also there will be a `.kismet` file in your home directory.
## Installing CYT

### Clone the repo

```bash
https://github.com/ArgeliusLabs/Chasing-Your-Tail-NG.git
```


### Installing requirments
Enter its folder:

```bash
cd "Chasing-Your-Tail-NG"
```

Create venv:

```bash
python3 -m venv .venv
```

Activate it:

```bash
source .venv/bin/activate
```

Installing requirements in `.venv`:


```bash
pip3 install -r requirements.txt
```

### Config.json

```bash
nano config.json
```

Change `kismet_logs` to be `/home/<username>/*.kismet` due to kismet logs stored in the user's home directory.

### Security setup

Ensure you're in `.venv`:
```bash
# Migrate credentials from insecure config.json
python3 migrate_credentials.py

# Verify security hardening
python3 chasing_your_tail.py
# Should show: "🔒 SECURE MODE: All SQL injection vulnerabilities have been eliminated!"
```

To start it with a GUI:

```bash
python3 cyt_gui.py
```

And that's it.


