---
title: Turning a raspberry pi pico into a BadUSB
date: 2026-03-10 00:00:00 +0000
categories: [flipper]
tags: [non technical]     # TAG names should always be lowercase
description: Guide to turning a raspberry pi pico into a BadUSB
toc: true
---

## Introduction
This guide covers how to convert a raspberry pi pico into a BadUSB. We will be using this github repo (<https://github.com/dbisu/pico-ducky>)

## Prerequisites
A raspberry pi pico ,jumper wires and a project board.

## Reset the flash memory
Firstly, we have to wipe its flash memory to start it on a clean slate. Hold down the boot button and connect it to your computer

A new device should now be seen on your file explorer:

![screenshot of new device](assets/posts/post10/post10_img1_newdevice.png)

Afterwards, copy `flash_nuke.uf2` (<https://datasheets.raspberrypi.com/soft/flash_nuke.uf2>) into it, this will wipe everything. 

![screenshot of nuke](assets/posts/post10/post10_img4_copy.png)

The device will now disappear for a while and will soon be visible again on your file explorer app. 


## Download the correct zip file
Under the `releases` section, download the correct zip file:
![screenshot of releases](assets/posts/post10/post10_img5_releases.png)


## Installing CircuitPython
Secondly, we will now install CircuitPython onto it , i will be using `adafruit-circuitpython-raspberry_pi_pico2_w-en_US-9.2.1.uf2` as i am using a raspberry pi pico 2w. 


Afterwards, your pico will now be called `CIRCUITPY` with its content being:

![screenshot of releases](assets/posts/post10/post10_img6_circuitpy.png)

## Copying the library files over
Extract the contents of the zip you downloaded earlier on. Now copy `/lib` over to the raspberry pi pico. Its content afterwards will be:

![screenshot of lib folder content](assets/posts/post10/post10_img7_lib.png)

## Modifying secrets.py (Only for Pico W)
Modify `secrets.py` to customise the name and password of the access point created. You will need to use this to upload and run scripts. 

![screenshot of content](assets/posts/post10/post10_img8_secrets.png)


## Copying the python files over
Copy over all `*.py` files to the pico. Its content afterwards will be:

![screenshot of content](assets/posts/post10/post10_img9_py.png)

## Modifying your payload
Open `payload.dd` on any text editor. This follows the same syntax as the one covered here: <https://bnuy621.github.io/posts/FlipperBadKB/> being Rubber Ducky scripting language 1.0. For demostration purposes, I will be using this simple script:

```bash
DELAY 500
GUI T
STRING HELLO WORLD
```
Windows version:

```bash
DEFAULT_DELAY 500
GUI R
REM this runs the 'run' program
```

After ensuring `payload.dd` is on your pico, unplug it from your device.

## Disable USB (Pico ONLY)
This will prevent it from being shown as an external device when plugged into your computer. This is already disabled on the Pico W. Shown on the Github repo:

![screenshot of content](assets/posts/post10/post10_img12_setup.png)


## Running the script
Now plug the device back in and the script will run automatically:

![screenshot of result ](assets/posts/post10/post10_img10_result.png)

## Stop the payload running automatically
This is the same for both the Pico and Pico W. If you want to configure the payload afterwards, have a jumper wire connect pin 1 to 3 which will stop it from running the script automatically. As shown on the Github repo:

![screenshot of content](assets/posts/post10/post10_img11_setup.png)

## Configuring your script (Pico W)
Connect to the access point configured earlier on in 'secrets.py'. You should see it under the list of avaiable Wifi networks:

![screenshot of content](assets/posts/post10/post10_img13_AP.png)

After connecting to it using the credentials configured earlier on, go to 192.168.4.1:

![screenshot of content](assets/posts/post10/post10_img14_ducky.png)

Click on 'edit' to edit 'payload.dd':

![screenshot of content](assets/posts/post10/post10_img15_edit.png)

I will modify it to say "HELLO RUBICON" instead:

![screenshot of modified script](assets/posts/post10/post10_img16_edited.png)

Click on 'submit' to submit the changes. 

![screenshot of submit changes](assets/posts/post10/post10_img17_editwork.png)


## Running the script (PicoW)
At the home screen, click on run to run the script:

![screenshot of result](assets/posts/post10/post10_img18_results.png)
