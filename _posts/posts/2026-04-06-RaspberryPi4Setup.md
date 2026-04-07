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
A LCD screen
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
Now upgrade the system on the raspberry pi 4:

```bash
sudo apt update && sudo apt full-upgrade -y
sudo reboot
```

(img of upgrade progress from phone)



## LCD screen
Now shut down the Pi 4 and attach the LCD screen. Power it back on afterwards. The screen will be blank for now.

These commands are taken from here: <https://www.waveshare.com/wiki/3.2inch_RPi_LCD_(B)>

### Configure driver

Open bash and run:

```bash
sudo apt-get install unzip -y
sudo apt-get install cmake -y
wget https://files.waveshare.com/wiki/common/Waveshare32b.zip
unzip ./Waveshare32b.zip
sudo cp waveshare32b.dtbo /boot/overlays/
```
To edit config.txt:

```bash
Edit config.txt file
```

Block these statements:

```bash
# Enable DRM VC4 V3D driver
#dtoverlay=vc4-kms-v3d
#max_framebuffers=2
```

Add these:

```bash
dtparam=spi=on
dtoverlay=waveshare32b
hdmi_force_hotplug=1
max_usb_current=1
hdmi_group=2
hdmi_mode=87
hdmi_cvt 320 240 60 6 0 0 0
hdmi_drive=2
display_rotate=0
```

### Set auto-start on boot

Edit `.bash_profile`:

```bash
sudo nano ~/.bash_profile
```

Add:

```bash
export FRAMEBUFFER=/dev/fb1
startx  2> /tmp/xorg_errors
```

Edit `99-fbturbo.~ file`:

```bash
sudo nano /usr/share/X11/xorg.conf.d/99-fbturbo.~
```

Add:

```bash
Section "Device"
        Identifier      "Allwinner A10/A13 FBDEV"
        Driver          "fbturbo"
        Option          "fbdev" "/dev/fb0"

        Option          "SwapbuffersWait" "true"
EndSection
```

### Set CLI auto-login

```bash
sudo raspi-config nonint do_boot_behaviour B2
sudo raspi-config nonint do_wayland W1
sudo reboot
```

After rebooting, UI will now be on the LCD. However, touch is still not implemented yet.

(img of it from phone)

### Configure touch
Yea no just dont as its bad either way


## Configure wifi

### Why?
You might be asking why buy ANOTHER wifi adapter when it already comes with one. The answer being the built-in one does NOT support monitor mode and that is pretty much essential for running the `Chasing your tail` project.


### How
I will be following this guide <https://github.com/lwfinger/rtw88>


Afterwards, reboot and run `lsmod | grep -i rtw`. You will see the drivers

(img of drivers from phone)


## Installing requisites for Chasing your tail

### Python
Most will have python installed. Run `python3 --version` to ensure it is above `3.6`.


### Kismet
I will be following this guide <https://www.kismetwireless.net/docs/readme/installing/linux/>


When cloning kismet, it will take a while

(img from phone)


When compiling it, it will take a long time 

(img from phone)

Installing:

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

## Installing CYT

### Clone the repo

```bash
https://github.com/ArgeliusLabs/Chasing-Your-Tail-NG.git
```

This will take a while:

(img from phone)

Cd:

```bash
cd "Chasing-Your-Tail-NG"
```
### Installing requirments
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
Ensure you're in `.venv`:

(img from phone)

### Security setup

```bash
# Migrate credentials from insecure config.json
python3 migrate_credentials.py

# Verify security hardening
python3 chasing_your_tail.py
# Should show: "🔒 SECURE MODE: All SQL injection vulnerabilities have been eliminated!"
```

You will get this error due to not having properly configured it:

(img from phone)

### Config.json

We will now proceed to start `kismet` once to load the files.

