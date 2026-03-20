---
title: "Flipper zero: BT - PC Monitor"
date: 2026-03-20 00:00:00 +0800
categories: [flipper]
tags: [bluetooth]    
description: 
toc: true
---

## Introduction
This is a guide to setting up Flipper zero PC monitor, an app that displays your devices's performance on your flipper zero. This is unstable for Linux users.

## Installing the app on flipper zero

Firstly, ensure your flipper zero is connected to your computer via an USB cable. Secodnly, go to this link <https://lab.flipper.net/apps/pc_monitor> and click `install`:

![pop up](assets/posts/post24/post24_img1_notconnected.png)

Select your device and click on `connect`:

![connect](assets/posts/post24/post24_img2_connect.png)

After installing, flipper lab will display this:

![installed on flipper](assets/posts/post24/post24_img3_installedflipper.png)

## Installing the backend app on your computer


Go to <http://github.com/TheSainEyereg/flipper-pc-monitor-backend/releases> and download the executable file.
Afterwards, run it:

![installed on flipper](assets/posts/post24/post24_img4_runapp.png)

## Pair your flipper with computer via Bluetooth
Enable bluetooth on the flipper and pair it with your computer.

If you're on Linux, run:
```bash
bluetooth ctl
scan on
pair <flipper MAC address>
```

After a while, if it pairs this should show up :

![installed on flipper](assets/posts/post24/post24_img5_result.png)

Image taken from: <https://github.com/TheSainEyereg/flipper-pc-monitor-backend/raw/master/.github/screenshots/app.png>

## Conclusion
This is not a practical way to go about viewing your computer's performance. 