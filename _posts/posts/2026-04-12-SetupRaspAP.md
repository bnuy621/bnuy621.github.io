---
title: "Setting up RaspAP"
date: 2026-04-10 00:00:00 +0800
categories: [raspberry pi]
tags: [non technical]    
description: 
toc: true
---

## Introduction
This guide serves to provide instructions to setting up RaspAP


## Prerequisites
1. one external wifi USB adapter (used to connect to the actual LAN)
2. built-in wifi adapter (used to HOST the AP)

## How
I will be using the instructions stated here <https://github.com/RaspAP/raspap-webgui>

### 1 - Setting localisation
This must be the country you are currently staying at. 

```bash
sudo raspi-config
```

Go to `localisation options` > `WLAN Country Set` > choose the country you are in. `Up` and `down` arrow keys to select your choice with the `left` and `right` ones to select `save` or `cancel`.


![](assets/posts/post34/img0_initconfig.png)

![](assets/posts/post34/img1_countries.png)

### 2  - Downloading RaspAP

Now run this command:

```bash
curl -sL https://install.raspap.com | bash
```

![](assets/posts/post34/img2_installcmd.png)

Now reboot the pi. Make sure your Pi is NOT connected to a network. If it is, this wont work. These commands will help in restarting the service for the AP:

```bash
sudo systemctl status hostapd
sudo systemctl restart hostapd
```

### 3  - COnfiguring it
After you are connected to the AP:

![](assets/posts/post34/img4_raspap.png)

Go to `http://10.3.141.1/` and login. You can also go to `localhost` on the pi itself

![](assets/posts/post34/img5_raspAPlogin.png)


Refresh the page if it errors out.

![](assets/posts/post34/img6_raspAP_login.png)

You will now be greeted by the dashboard:

![](assets/posts/post34/img7_raspAPdashboard.png)

### Hotspot basic

Go to `Hotspot` > `basic` to configure your AP. 
![](assets/posts/post34/img8_raspAP_hotspot_basic.png)

Ensure it iis using wlan0 which should be the built in wifi adapter

Go to `Hotspot` > `security` to change the password for your AP.

![](assets/posts/post34/img9_raspAP_hotspot_security.png)

### 4 - Configuring repeater (pain)

Alright now for the real deal, configuring repeater mode. Firstly, go the `networking` tab and note down the metric values for your `wlan` interfaces.

![](assets/posts/post34/img3_raspAP_networktab.png)

The USB (wlan1) is 601 while the default (wlan0) is 600

Go to `DHCP server` and enter the metric value for `605`: (must be larger than wlan1)

Restart the service and you should be able to connect to the AP while having a stable but slow internet connection

If you cant connect and seem to fail at the authentication part, run these commands. (note i am using arch linux so these commands might differ if you are using another OS):

View live logs:

```bash
sudo journalctl -f -u NetworkManager -u wpa_supplicant
```

Forget password and replace with correct:
```bash
nmcli connection delete "<ap_name>"                                   
nmcli device wifi connect "<ap_name>" password "<password"
```

To prevent it from auto start on boot, i just disabled the `hostapd` service

```bash
sudo systemctl disable hostapd
```

To start:

```bash
sudo systemctl enable hostapd
sudo systemctl start hostapd
```

### 5 - my baseline
My current working configuration (baseline) is this:

![](assets/posts/post34/img10_raspAP_baseline1.png)

![](assets/posts/post34/img11_raspAP_baseline2.png)

![](assets/posts/post34/img12_raspAP_baseline3.png)

![](assets/posts/post34/img13_raspAP_baseline4.png)

![](assets/posts/post34/img14_raspAP_baseline5.png)

So if i mess up later i will revert to this.