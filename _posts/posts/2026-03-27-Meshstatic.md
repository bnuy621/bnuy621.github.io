---
title: Setting up meshstatic for communication
date: 2026-03-27 00:00:00 +0800
categories: [misc]
tags: [non technical]     
description: 
toc: true
---

## Introdcution
This post aims to show how to setup meshtastic for communication. 

## Prerequisites
Have meshtastic flashed on your LoRa device. In this case, i am using the LilyGo T-echo. (<https://lilygo.cc/en-us/products/t-echo-lilygo>)

![devices](assets/posts/post29/post29_img8_devices.png)


## What is meshtastic
Meshtastic is an off-grid communication platform which converts LoRa devices into a decentralised mesh for communication. More informaton on it here: <https://meshtastic.org/>

### Steps 

### Step 1 - Downloading the app
Download the meshtastic app onto your device (<https://meshtastic.org/docs/software/android/installation/>) and grant it permissions to use `Bluetooth` as well as `find and locate nearby devices`. 

![devices](assets/posts/post29/post29_img10_app.png)


### Step 2 - Pairing with your LoRa device
Turn on your meshtastic radio and you'll see it appear under list of devices:

![device shown](assets/posts/post29/post29_img1_deviceshows.png)


Click on it and enter the PIN shown on the meshtastic radio device to pair with it. If successful, your phone will now be connected to your meshstatic radio:

![device successfully connected](assets/posts/post29/post29_img2_connected.png)


We will now proceed to configuring it

### Step 3 - Configuring the device's region
This is very important as the device needs to know which frequency it is allowed to operate on with this differing per country. Configure the `region` to where you are at and click `save`. The device will now proceed to reboot.


### Step 4 - Creating a channel

Go to your meshtastic radio's setttings and click on channels:

![device settings](assets/posts/post29/post29_img3_settings.png)


Create your channel:

![device settings](assets/posts/post29/post29_img4_channel.png)

Go to your `conversations` tab and you will see your new channel. The green lock symbol means that channel is encrypted:

![device settings](assets/posts/post29/post29_img5_greenbtn.png)

### Step 5 - Sharing the channel

Click on the green button at the botton right and select `share channels QR code`:

![device settings](assets/posts/post29/post29_img6_greenbtnoptions.png)

Proceed to click on `Generate QR Code` which will generate a QR code for your channels:

![device settings](assets/posts/post29/post29_img9_genQRcode.png)

To add this channel on another device, go back to the `coversations` tab and click on the green button. Afterwards, select `scan shared QR code`. After scanning the QR code, select `add` and you will now see the newly created channel in your `conversations` tab.


### Results:
Messaging each other will now work even without wifi:

![meshstatic working](assets/posts/post29/post29_img7_results.png)
