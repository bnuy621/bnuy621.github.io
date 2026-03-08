---
title: How to use mautrix-discord
date: 2026-03-04 12:00:00 +/-TTTT
categories: [matrix]
tags: [non technical] 
description: Guide to using mautrix-discord
toc: true
---

## Introduction
This post will cover how to use mautrix-discord. 

## Prerequisites
mautrix-discord is set up, if you have not refer to this post(<https://bnuy621.github.io/posts/SetupMautrixDiscord/>)

## Setting up the discord bot
Firstly, login to the discord developer portal using your discord account (<https://discord.com/developers/applications>). It is best to have 2FA enabled via your authenticator app for this account. After logging in, you should see a button "New Application":

![screenshot of create new app](assets/posts/post5/post5_img2_createnewapp.png)

Secondly, on the sidebar go to 'Bot' and click on 'reset token' to get your bot's access token:
![screenshot of bot reset token](assets/posts/post5/post5_img3_resettoken.png)

Thirdly, scroll down and allow all intents:
![screenshot of bot intents](assets/posts/post5/post5_img4_intents.png)

Fourthly, on the sidebar go to OAuth2 and select 'bot' under OAuth2 URL generator.
![screenshot of bot Oauth2 ](assets/posts/post5/post5_img5_auth.png)

Fifhtly, configure the permissions for your bot, in this case i gave it administrator permissions
![screenshot of bot Oauth2 permissions](assets/posts/post5/post5_img6_perms.png)

Next, use the link generated to invite your bot to your discord server.

## Setting up relaying/ bridging for a channel
We will be using a discord bot in this case. Firstly, copy its access token and run the following command in the matrix room

```bash
!discord login-token bot <bot_token_here>
```

If successful, this will be shown:

![screenshot of bot token login successful](assets/posts/post5/post5_img1_bottokenworks.png)

Run the following to ensure it is logged in as the correct user:

```bash
!discord ping
```
Run the following to view the servers your bot is in:

```bash
!discord guild status
```

Next, run the following to link your matrix room to a channel. To get the server and channel ID, refer to the URL in your address bar

```bash
https://discord.com/channels/SERVER_ID/CHANNEL_ID
```

```bash
!discord create-portal <CHANNEL_ID>
```
A room will be automatically created with the name of your channel. It will follow the naming pattern of "#channel_name".

![screenshot of channel to room portal created](assets/posts/post5/post5_img7_room.png)

Messages from discord will be sent to matrix room and vice versa.

![screenshot of messages from discord to matrix](assets/posts/post5/post5_img8_matrix.png)

![screenshot of messages from matrix to discord](assets/posts/post5/post5_img9_discord.png)

## Setting up relaying/ bridging for a server
Ensure your matrix bot is logged in, afterwards run this command:

```bash
!discord guild bridge SERVER_ID --entire

```
This will create a space on matrix with the same structure as your discord server with messages being relayed between Discord and Matrix

![screenshot of Discord server's structure](assets/posts/post5/post5_img10_DSERV.png)

![screenshot of Matrix space's structure](assets/posts/post5/post5_img11_MSPACE.png)
