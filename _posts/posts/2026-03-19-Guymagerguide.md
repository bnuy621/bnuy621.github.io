---
title: "Guymager guide: Getting an E01 image"
date: 2026-03-19 00:00:00 +0800
categories: [forensic]
tags: [tools]    
description: 
toc: true
---

## Introduction
This guide breifly covers how to use Guymager to get an `E01` image of your disk

## Steps

### Step 1
Firstly, right click on the device you're imaging:

![acquire image](assets/posts/post22/post22_img1_acquire.png)


### Step 2
Secondly, select `expert witness format`:

![E01](assets/posts/post22/post22_img2_E01.png)

`split size` refers to the maximum size of each `E01` file. 

### Step 3
Thirdly, fill up on the details accordingly:

![E01](assets/posts/post22/post22_img3_details.png)

### Step 4
Fourthly, enter the file path where the `E01` files will be stored:

![E01](assets/posts/post22/post22_img4_filepath.png)

### Step 5
Fifthly, enter the name for the `E01` file. The `image filename` and `info filename` will be the exact same:

![](assets/posts/post22/post22_img5_filenames.png)

### Step 6
Sixthly, tick `calculate MD5` and `verify image after acquisition`. This is to ensure the image is not corrupt and an exact image of the device:

![hashing options](assets/posts/post22/post22_img6_hashes.png)

Now click run and the status of the device will be `running`:

![status - running](assets/posts/post22/post22_img7_running.png)

After a while, Guymager will show this:

![status - verified](assets/posts/post22/post22_img8_status2.png)

## Results

![status - verified](assets/posts/post22/post22_img9_results.png)
