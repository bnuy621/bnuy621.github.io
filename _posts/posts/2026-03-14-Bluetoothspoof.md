---
title: "Flipper zero BadKB: BLE mode"
date: 2026-03-14 00:00:00 +0800
categories: [flipper]
tags: [non technical]    
description: Guide to configuring BLE for BadKB on flipper zero
toc: true
---

## Introduction
Today i will be talking about BLE mode when using BadKB on the flipper zero

## Prerequisites
A flipper zero running the momentum firmware

## What does it do
Earlier on when we wanted to execute a script on the device, we needed to physically connect the flipper zero to it via a USB cable. If you want it to be more discreet, you could run it via Bluetooth. 

## Configuring it
Firstly, select your script:

![script UI](assets/posts/post11/post11_img2_selectscript.jpg)

Next click on the left arrow key,  you will see a Bluetooth Icon:

![script UI](assets/posts/post11/post11_img1_scriptUI.jpg)


## Configurations - Keyboard mode
This changes the keyboard the BadUSB is using to suit the one your device is running:

![keyboard hovered](assets/posts/post11/post11_img3_keyboard.jpg)

![keyboards](assets/posts/post11/post11_img4_keyboards.jpg)


## Configurations - Persist pairing
This causes the BadUSB to keep trying to pair with your device:

![persist pairing hovered](assets/posts/post11/post11_img5_persistpairing.jpg)


## Configurations - Pairing mode
This changes the way pairing is done. There are 3 modes being:

Yes/No:

![persist pairing hovered](assets/posts/post11/post11_img6_pairM_YN.jpg)


Auth:

PIN type:

![persist pairing hovered](assets/posts/post11/post11_img7_pairM_PINtype.jpg)

PIN Y/N:

![persist pairing hovered](assets/posts/post11/post11_img8_pairM_PINYN.jpg)

![persist pairing hovered](assets/posts/post11/post11_img16_PINYN_AUTH.png)

## Configuration - Set device name
This allows you to change the name to something more legitimate such as that of a bluetooth keyboard:

![persist pairing hovered](assets/posts/post11/post11_img9_devicename.jpg)


In this case, i will be changing it to keychron k8:

![persist pairing hovered](assets/posts/post11/post11_img10_newename.jpg)



## Configuration - MAC address
This allows you to change the MAC address to be the exact same of the device you're trying to impersonate:

![persist pairing hovered](assets/posts/post11/post11_img12_setMAC.jpg)

In this case i will be changing it to be the same MAC of my wireless keyboard:

![persist pairing hovered](assets/posts/post11/post11_img13_newname.jpg)

There is also an option to randomise the MAC address:

![persist pairing hovered](assets/posts/post11/post11_img11_randomMAC.jpg)

## Result

BadUSB bluetooth device details:

![persist pairing hovered](assets/posts/post11/post11_img15_newdevice.png)


Script running:

![persist pairing hovered](assets/posts/post11/post11_img17_endresult.png)
