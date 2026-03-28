---
title: Flipper zero BadKB
date: 2026-03-08 00:00:00 +0800
categories: [flipper]
tags: [non technical]     # TAG names should always be lowercase
description: Guide to using the BadKB function on a flipper zero
toc: true
---

## Introudction
This guide covers how to use the BadKB (Bad Keyboard) function on the flipper zero.

## Prerequisites
Have momentum installed on your flipper zero as i will be using that for today's guide If you have not refer to this guide: <https://bnuy621.github.io/posts/FlipperCustom/>.

## How does it work?
Taken from a comment on a reddit post (<https://www.reddit.com/r/flipperzero/comments/xdktd1/how_does_a_bad_usb_work/>), 'BadUSB is a function that allows the flipper to emulate a keyboard. It then emulates the keys typed onto the keyboard with keys typed depending on the script you use.'

## Making your own script
I will be making a simple script that types the phrase 'HELLO WORLD' on my terminal 4 times. Firstly, create a text file called 'hello_world.txt':

![screenshot of file](assets/posts/post8/post8_img1_nano.png)

The scripting language used will be the Rubber Ducky Scripting Language 1.0 (<https://web.archive.org/web/20220816200129/http://github.com/hak5darren/USB-Rubber-Ducky/wiki/Duckyscript>). This is the script:

```bash
DEFAULT_DELAY 500
GUI T
REM this presses the SUPER key with T afterwards.
STRING HELLO WORLD
REPEAT 4
REM this repeats the previous command 4 times.
```

## How the script works

DEFAULT_DELAY sets a delay of X milliseconds between each and every command. For example:

```bash
DEFAULT_DELAY 1000
STRING H
STRING I
```

This causes a one second delay between typing 'H' and 'I'. 

GUI refers to your SUPER / windows Key. Both are the same with different names depending on the operating system used.

```bash
GUI R
```
This is the equivalent to pressing WIN + R which will open up 'run' on windows.

STRING refers to the keys you want to press. For example:

```bash
STRING HELLO
```
Types out 'HELLO'

REM is the equivalent to a comment from any programming language. Anything after REM on the same line will not be typed or interpreted as keystrokes.

```bash
REM this is a comment
```

REPEAT repeats the previous command X number of times. In this case, REPEAT 4 causes the command to type 'HELLO WORLD' repeat four times.

```bash
STRING HELLO WORLD
REPEAT 4
```


## Uploading the script
Open up qflipper, click on the file icon, go to the 'badusb' folder and upload your text file there:

![screenshot of upload file](assets/posts/post8/post8_img2_upload.png)

## Running the script 
On your flipper, click on the round button:

![screenshot of apps ](assets/posts/post8/post8_img3_apps.png)

scroll down till you see 'BadKB':

![screenshot of Bad KB](assets/posts/post8/post8_img4_BadKB.png)

Click the round button and scroll using the arrow buttons till you see your file. In this case it is 'hello_world':

![screenshot of script](assets/posts/post8/post8_img5_script.png)

The default method to run the script is via USB cable. Click on the round button to use this script and click again on it again to run the script. After it is done running, you will see 100% on the screen which means the script finished running without any errors.

![screenshot of script finished running](assets/posts/post8/post8_img6_result.png)


## Conclusion
This is a simple guide on how to use the BadUSB/ BadKB function. It is not a must to own a flipper to have a BadUSB. Instead, using a raspberry pi pico works too (<https://github.com/dbisu/pico-ducky>)