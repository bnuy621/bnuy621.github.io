---
title: Making a custom flipper zero wallpaper
date: 2026-03-01 12:00:00 +/-TTTT
categories: [flipper]
tags: [non technical]     # TAG names should always be lowercase
description: Guide to making a custom flipper zero wallpaper
toc: true
---
## Introduction
This guide will cover how to make your own flipper zero wallpaper. You will need Git installed first.
![screenshot of qflipper](assets/posts/post2/post2_img15_flipper.png)

## Prerequisites
A flipper zero, qflipper and Git installed.

## Select your wallpaper
Firstly, select your wallpaper. In this case i chose an Elysia live wallpaper:

![screenshot of elysia wallpaper](assets/posts/post2/post2_img1_elysiawallpaper.png)

## Convert your wallpaper
Secondly, go to <https://ezgif.com/> to format your file to a GIF format:

![screenshot of convert file to gif format](assets/posts/post2/post2_img2_convertgif.png)

## Resizing the wallpaper
Thirdly, resize the wallpaper to be 128 x 64 so that it fits on the flipper screen:

![screenshot of resized gif](assets/posts/post2/post2_img3_resize.png)

## Applying filters
Fourthly, apply the grayscale filter and save the GIF:

![screenshot of grayscale filter on](assets/posts/post2/post2_img4_filter.png)

## Split into individual frames

Now, split the GIF into individual images in the PNG format. Take note of the end and start time as it is not set to the full length of the video by default.  Afterwards, download the frames as a ZIP file and extract it.

![screenshot of duration](assets/posts/post2/post2_img5_duration.png)

![screenshot of frame output](assets/posts/post2/post2_img6_frameoutput.png)

## Rename the files
Afterwards, rename the files to be 'frame_0.png, frame_1.png, ...' :

![screenshot of frame images](assets/posts/post2/post2_img7_frames.png)

## Create meta.txt
Next, create meta.txt which provides the metadata about this animation such as number of frames, frame rate, etc. There are more ways to customise the animation but that is not covered here:

![screenshot of meta txt](assets/posts/post2/post2_img8_metatxt.png)

## Cloning the flipper zero repo
Now, create a folder and clone the official flipper zero repo (https://github.com/flipperdevices/flipperzero-firmware.git):

```bash
git clone https://github.com/flipperdevices/flipperzero-firmware.git
```

![screenshot of repo](assets/posts/post2/post2_img9_repo.png)

Afterwards, move your file containing the images to '/flipperzero-firmware/assets/dolphin/external':

![screenshot of external folder](assets/posts/post2/post2_img10_external.png)

## Editing manifest.txt
Next, edit manifest.txt to include your animation. Take note that the name of your animation must be the same as the folder:

![screenshot of manifest](assets/posts/post2/post2_img11_manifesttxt.png)

![screenshot of external](assets/posts/post2/post2_img12_external.png)

## Compiling the animation
Now run the command './fbt icons proto dolphin_internal dolphin_ext resources'
![screenshot of assets complete](assets/posts/post2/post2_img13_compile.png)

Your animation will now be located at '/flipperzero-firmware/build/f7-firmware-D/assets/compiled/dolphin'
![screenshot of assets location](assets/posts/post2/post2_img14_eassets.png)

## Uploading the animation
Run qflipper , click on the file icon:

![screenshot of assets location](assets/posts/post2/post2_img16_flipperfileUI.png)

Afterwards, click on the 'dolphin' folder:

![screenshot of assets location](assets/posts/post2/post2_img17_dolphinUI.png)

Lastly, drag the folder containing your animation into the 'dolphin' folder and the updated manifest.txt here. Your animation should now be playing:

![screenshot of qflipper](assets/posts/post2/post2_img15_flipper.png)
