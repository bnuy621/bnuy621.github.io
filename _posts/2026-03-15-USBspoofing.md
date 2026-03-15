---
title: "Flipper zero BadKB: USB mode"
date: 2026-03-15 12:00:00 +/-TTTT
categories: [flipper]
tags: [non technical]    
description: Guide to configuring USB mode
toc: true
---

## Introduction
Today i will be talking about USB mode when using BadKB on the flipper zero

## Prerequisites
A flipper zero running the momentum firmware

## Configuring it
Firstly, select your script:

![script UI](assets/posts/post11/post11_img2_selectscript.jpg)

Afterwards, click on the left arrow key till you see a USB icon:

![script UI](assets/posts/post13/post13_img11_selectscript.jpg)

## Configurations - Keyboard mode
This changes the keyboard the BadUSB is using to suit the one your device is running:

![keyboard hovered](assets/posts/post13/post13_img1_hoverKB.jpg)

![keyboards](assets/posts/post13/post13_img2_KBlist.jpg)

## Configurations - Manufacturer name
This changes the manufacturer name of the BadUSB. I will be changing it to `Rubicon`:

![configure manufacturer name](assets/posts/post13/post13_img3_setManName.jpg)

![edit manufacturer name](assets/posts/post13/post13_img4_editManName.jpg)



## Configurations - Product name
This changes the product name of the BadUSB. I will be changing it to `Coral`:

![configure manufacturer name](assets/posts/post13/post13_img5_setProdName.jpg)

![configure manufacturer name](assets/posts/post13/post13_img6_editProdName.jpg)


To view the manufacturer and product name of the USB devices connected on Linux, run this command:

```bash
lsusb
```

```bash
Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
Bus 001 Device 002: ID 046d:c548 Logitech, Inc. Logi Bolt Receiver
Bus 001 Device 009: ID e1e9:f12d Rubicon Coral
```

As we can see here, our BadUSB's manufacturer name is `Rubicon` with `Coral` as its product name.

## Configurations - VID and PID

VID stands for vendor product ID while PID stands for product ID. More info on this site: <https://docs.espressif.com/projects/esp-iot-solution/en/latest/usb/usb_overview/usb_vid_pid.html>. We can configure this to make our BadUSB look more legitimate:

![configure VID and PID](assets/posts/post13/post13_img7_setVIDPID.jpg)

![edit VID and PID](assets/posts/post13/post13_img8_editVIDPID.jpg)

You can also randomise them:

![randomise VID and PID](assets/posts/post13/post13_img9_randomiseVIDPID.jpg)

To view the VID and PID of the USB devices connected on Linux, run the following command:
```bash
lsusb
```

```bash
Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
Bus 001 Device 002: ID 046d:c548 Logitech, Inc. Logi Bolt Receiver
Bus 001 Device 009: ID e1e9:f12d Rubicon Coral
```

The VID is `ele9` while the PID is `f12d`.



