---
title: "Malware analysis: Duet night abyss"
date: 2026-03-18 00:00:00 +0800
categories: [malware analysis]
tags: []    
description: 
toc: true
---

## Introduction
Lately there has been rumours of the DuetNightAbyss launcher/game installing malware itself onto your device. This post aims to find out if that is true. 

## Hashing

### Initial launcher
Commands used:

```bash
Get-FileHash -Path "C:\Users\rober\Downloads\DNA_20260210064451_15.0.0_pub_Launcher_Installer.exe" -Algorithm MD5

Get-FileHash -Path "C:\Users\rober\Downloads\DNA_20260210064451_15.0.0_pub_Launcher_Installer.exe" -Algorithm SHA256

```
MD5:
![offset](assets/posts/post22/post22_img1_launcher_MD5.png)

SHA256
![offset](assets/posts/post22/post22_img2_launcher_SHA256.png)
