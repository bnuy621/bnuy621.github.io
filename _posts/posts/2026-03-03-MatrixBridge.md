---
title: Bridging matrix to  discord
date: 2026-03-03 00:00:00 +0800
categories: [matrix]
tags: [technical]
description: Guide to bridging matrix to discord
toc: true
---
## Introduction
This guide serves to help you connect your matrix and discord communities. We will be using <https://t2bot.io/discord/> for this as it is the most straightforward solution.

## Creating a matrix account
Go to <https://app.element.io/#/register> and create a Matrix account, keep the default homeserver as 'matrix.org'. Afterwards, enter your account details and you should be logged in.

![screenshot of element create account page](assets/posts/post3/post3_img1_createacc.png)

![screenshot of element create account page](assets/posts/post3/post3_img2_createacc2.png)

## Inviting the bot to your Matrix room
Go to <https://t2bot.io/discord/> , click on the link (<https://matrix.to/#/@_discord_bot:t2bot.io>) shown below, this will create a Matrix room with you and the bot.

![screenshot of t2bot instructions](assets/posts/post3/post3_img3_invite.png)

![screenshot of t2bot instructions](assets/posts/post3/post3_img4_matrixroom.png)

## Inviting the bot to your discord server
Now, you will want to invite the bot to your discord server. Ensure that you are administrator on that server. After the bot has joined, go back to the Matrix room.

![screenshot of t2bot invite](assets/posts/post3/post3_img5_matrixbot.png)

![screenshot of t2bot invited](assets/posts/post3/post3_img6_botinvited.png)

## Setting up the bridge
Go back to your discord channel In the address bar there should be a URL similar to this : 

```bash
https://discordapp.com/channels/ServerID/ChannelID
```
use that as a reference to enter the command to bridge your channel to your Matrix room:

```bash
!discord bridge ServerID ChannelID. 
``` 

The bot wil now request for permission to setup this bridge on your discord server. Go back to discord and you should see it asking for permission. Enter '!matrix approve' , the bot will now work afterwards and messages from discord will be shown in your matrix room.

![screenshot of matrix bot approval](assets/posts/post3/post3_img8_matrixapprove.png)

![screenshot of matrix bridge confirmed](assets/posts/post3/post3_img9_bridgeconfirmed.png)

![screenshot of matrix bridging works](assets/posts/post3/post3_img10_messageworks.png)
