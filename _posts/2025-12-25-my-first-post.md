---
title: "The USB"
date: 2025-12-25
categories: [blog]
tags: [welcome]
---

Your content here. You can use **Markdown** formatting.

## Introduction
Hi there, this is my first blog and shot at making one about digital forensics.
Today we will be looking how the options selected in rufus affects the structure of an USB.We will be covering two samples - FOX621_SAMPLE1. It was formatted via rufus with the following options:
- Boot selection: Non Bootable
- Partition scheme: MBR
- File system: FAT32
- Cluster size: 8192 bytes

## Boot selection
As we have chosen ‘Non Bootable’ This causes the first 446 bytes of the first sector to be empty (blue square). This can be seen below:

<img src="/assets/images/BLOG_1_USB/figure1_new.png" alt="MBR non-bootable" style="width:100%">

This screenshot proves that as we have selected it to be non bootable, this causes there to be no need of a primary boot loader which is supposed to take up the first 446 bytes of the MBR.
Now, what if we did select it to be bootable?

<img src="/assets/images/BLOG_1_USB/figure2_new.png" alt="MBR bootable" style="width:100%">

As seen in the screenshot above at the content in blue, if we do select it to be bootable, the first 446 bytes will have content in them as you can boot up from it.
Hence, we can state that selecting the option ‘Non Bootable’ in rufus causes the content in the first 446 bytes of the MBR of the USB to be nulled. 

## Partition scheme
To prove that we have chosen MBR and not GPT, we will be looking for the MBR signature which is 0x55AA at byte offset 0x1FE to 0x1FF. 

<img src="/assets/images/BLOG_1_USB/figure3_new.png" alt="MBR bootable" style="width:100%">


As we can see in the screenshot, above the MBR signature 0x55AA does exist at the correct location. To further prove this, the first partition entry of a disk using GPT will have byte offset 0x1C2 set to EE ( protective MBR ) which is not the case here as the byte offset is 0x0C. 

Hence, we can verify that the partition scheme here matches up with what we have chosen earlier on.
## File system
The fastest way to tell what file system it is using is by checking the partition type which is located at byte offset 0x4 at the first partition entry located at the MBR. We can also note here there’s only one partition for the entire USB.

<img src="/assets/images/BLOG_1_USB/figure4_new.png" alt="MBR bootable" style="width:100%">


As we can see in the screenshot above. The partition type is 0x0C which translates to FAT32. This matches up with what we chose earlier which is FAT32.
Hence, we can verify the file system matches up with what we have chosen earlier on.

## Cluster size
To verify the cluster size is actually 8192 bytes, we will be looking at the volume boot record (VBR) of our FAT32 file system. In particular, we are looking at byte offset 0x08 and 0x0D as they contain the number of bytes per sector as well as the number of sectors per cluster. This will allow us to calculate the actual cluster size and verify it is 8192 bytes.

<img src="/assets/images/BLOG_1_USB/figure5_new1.png" alt="MBR bootable" style="width:100%">


The value in red shows the number of bytes per cluster which is 0x0002 (512 bytes). The value in blue shows the number of sectors per cluster which is 0x10 (16). 16 x 512 = 8192. 
Hence we can verify that the cluster size is the same as what we have selected earlier on.