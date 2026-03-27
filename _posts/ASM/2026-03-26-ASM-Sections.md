---
title: ASM - Sections
date: 2026-03-26 00:00:00 +0800
categories: [ASM]
tags: [basics]     
description: 
toc: true
---

## Introductions
This post covers the three main sections in Assembly. These three sections are `.data`, `.bss` and `.text`

## .data section
This is where variables are declared. To declare it:

```yaml
section .data
```

For example:

```bash
section	.text
   

section	.data
msg db 'BNUY'
```

Compile it, afterwards use the following command to view the data in the `.data` section:

```bash
readelf -p .data data_bnuy
```

This dumps the `.data` of `data_bnuy` as strings. More info on readelf here: <https://man7.org/linux/man-pages/man1/readelf.1.html>

Results:

![screenshot of data_bnuy's .data](assets/posts/ASM/sections/ASM-img1-dumpdata.png)

## .bss section
Usually you will declare non zero variables in the `.data` sections while declaring variables with the value of zero in the `.bss` section. When ran, this causes to program to reserve 'X' amount of space in memory while not inflating the size of the executable itself.

For example:

```bash
section	.text
   

section	.data
resb 1000000
```

```bash
section	.text
   

section	.bss
msg resb 1000000
```
This causes the file size to differ:

![file size differ](assets/posts/ASM/sections/ASM-img2.png)

## .text section

This is the section where your actual code lies. Declare this section via:

```bash
section .text
```

Afterwards you will need to declare `global_start` to state the linker point where your code starts.

For example:

```bash
section	.text
   global _start     
	
_start:	            
   mov	edx, msg

section	.data
msg db 'BNUY621'
```

This code moves `BNUY621` to the `edx` register. 







