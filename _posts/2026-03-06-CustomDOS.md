---
title: How to make your own DOS sprite
date: 2026-03-06 12:00:00 +/-TTTT
categories: [Misc]
tags: [Terraria] 
description: Guide to making your own DOS sprite
toc: true
---
## Disclaimer
This post serves to only guide others how to modify a existing terraria mod. I do not encourage the plagarism of other people's mod using this guide.

## Introduction
Today i will be covering how to make your own sprite for the item 'Daawnlight Spirit Origin' from the Terraria Calamity Mod. This was done as i was not satisified with the new sprite which looks worse than the original (old on the left, new on the right):

![screenshot of DSO mod](assets/posts/post6/post6_img1_old_new.png)

## Extracting the sprite sheet
Firstly, we need the current sprite sheet so that we can alter it to our liking. This can be done via extracting the files from the mod, altering the files and recompiling the mod update it with our new sprite sheet.  In this case, i will be using the 'DSO sprite expansion mod'.

![screenshot of DSO mod](assets/posts/post6/post6_img3_DSOMOD.png)

To extract the files from a mod, download the mod 'Mod Extractor' and enable it. Afterwards, go to any of the mods, click on the question mark icon, click on 'extract':

![screenshot of mod extractor enabled](assets/posts/post6/post6_img4_modextractor.png)

![screenshot of button](assets/posts/post6/post6_img5_moreinfo.png)

![screenshot of extract buttons](assets/posts/post6/post6_img6_extract.png)

After extracting, your default file managing app should display the contents of the extracted content:

![screenshot of extracted content](assets/posts/post6/post6_img7_modcontent.png)

The sprite sheet itself is located at 'Content/Projectiles/Minions/':

![screenshot of extracted spritesheets](assets/posts/post6/post6_img8_spritesheets.png)

The icon for the buff is located at 'Content/Buffs/':

![screenshot of extracted buffs](assets/posts/post6/post6_img9_buffs.png)

## Editing the sprite sheets
Copy the images into a folder and tweak it to your liking. The frames on the left side is the idle animation while the one on the right side is the animation played when marking an enemy. I used Asesprite to edit mine: 

![screenshot of edit sprite](assets/posts/post6/post6_img13_editsprite.png)

![screenshot of edit buff](assets/posts/post6/post6_img14_editbuff.png)

## Recompiling the mod
Now, go back to the folder where the extracted content is stored , copy the folder containing the mod somewhere.

![screenshot of copied DSO folder](assets/posts/post6/post6_img15_DSOnewloc.png)

Afterwards, enter the copied folder and replace the respective sprite sheet. Now, go back to Tmodloader and click on "Develop Mods" and click on "Open sources". 

![screenshot of develop mods page](assets/posts/post6/post6_img18_devmods.png)

![screenshot of open sources folder](assets/posts/post6/post6_img19_opensources.png)


This will open up the folder containing your mods. Copy the altered mod folder into it and 'ModAssemblies' just to be safe:

![screenshot of copied DSO folder](assets/posts/post6/post6_img16_ModSources.png)

![screenshot of copied DSO folder](assets/posts/post6/post6_img17_ModAssemblies.png)

Go back to 'Develop mods' and click on 'build + reload'

![screenshot of open sources folder](assets/posts/post6/post6_img19_opensources.png)

Afterwards, load into the game and your custom DOS sprite will be there:

![screenshot of new sprite](assets/posts/post6/post6_img20_results.png)

Lastly, delete the mod folder from your ModSources folder so as to not accidentally publish it.

## Conclusion
This can be applied to pretty much modifying anything in a prexisting mod, just do not change the filename. 
