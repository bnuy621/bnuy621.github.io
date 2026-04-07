---
title: "Practical Malware analysis: Basic static malware analysis "
date: 2026-04-07 00:00:00 +0800
categories: [malware analysis]
tags: [lab]    
description: 
toc: true
---

## Introduction
Techniques and tools for basic static malware analysis


## 1 - Antiviruses
Antiviruses and websites such as <https://www.virustotal.com/> are great for knowing if the file is malicious. However, a brand new virus will not be detected as malware due to it not having a signature. I recommend virustotal as it also provides information such as file signature,hashes,import, section information as well as some dynamic analysis.



### How
Upload your file onto <https://www.virustotal.com/>. In this case i will be using `Lab01-01.exe`

![lab01-01.exe virustotal ](assets/posts/MA/lab1/MA-lab-1-1_img5_exeVT.png)

<https://www.virustotal.com/gui/file/58898bd42c5bd3bf9b1389f0eee5b39cd59180e8370eb9ea838a0b327bd6fe47>

Hashes:

![lab01-01.exe virustotal hashes ](assets/posts/MA/chap1/img12_VT_hashes.png)

Imports:
![lab01-01.exe virustotal imports](assets/posts/MA/chap1/img13_VT_imports.png)

Sections:
![lab01-01.exe virustotal sections](assets/posts/MA/chap1/img14_VT_sections.png)

Compile time:
![lab01-01.exe virustotal history](assets/posts/MA/chap1/img15_VT_history.png)



## 2 - Hashing
Hashing helps to verify the integrity of a file. This can be used in cases outside of basic static malware analysis such as comparing the file you downloaded against the hash value posted on its website. This allows you to verify if the file has been modified/corrupted.

### How 
This can be done in multiple ways.

#### CLI 
Linux:

```
md5sum <file_path>
```
![](assets/posts/MA/chap1/img1_md5.png)

Powershell:
```bash
Get-FileHash -Algorithm MD5 -Path <file_path>
```

#### DetectItEasy

Load the file into `DIE`, click on `hashes`

![](assets/posts/MA/chap1/img20_DIE_hashes1.png)



#### imhex

Load the file into `imhex`, click on `view` > `hashes`:

![](assets/posts/MA/chap1/img21_imhex_hashes1.png)

`Add hash` > i chose `MD5`

![](assets/posts/MA/chap1/img22_imhex_hashes2.png)

On the `hex editor` , right click and select all. This will output the hash of the entire file.

![](assets/posts/MA/chap1/img23_imhex_hashes3.png)

## 3 - Extracting strings
Extracting strings from a file provides further insight into what it does such as the URLS used. Take note that Unicode and ASCII store them differently. For example, B in ASCII is `0x42 0x00`. While in unicode it is `0x00 0x42 0x00 0x00`. Secondly, the `NULL` terminator for ASCII has 1 `00` while unicode has 2.

### How 


#### DetectItEasy AKA DIE

Firstly, load your file into `DIE`. 


![](assets/posts/MA/chap1/img2_DIE1.png)

Secondly, click on `advanced` on the right to use the advanced features.

![](assets/posts/MA/chap1/img3_DIE2.png)

Next, click on `strings` to view the extracted strings

![](assets/posts/MA/chap1/img4_DIE3.png)

#### imhex

Firstly, load your file into imhex

Pattern editor > `import pattern file`:

![](assets/posts/MA/chap1/img16_imhex1.png)

Select `Microsoft PE Portable Executable`:

![](assets/posts/MA/chap1/img17_imhex2.png)

Click on `View` > `Find` > `Search`

![](assets/posts/MA/chap1/img18_imhex3.png)

This will now output all strings found:

![](assets/posts/MA/chap1/img19_imhex4.png)



## 4 - Imported libraries
Viewing the imported libraries provides hints about what the malware does based on the functionality of the library itself. For example, the presence of `WSock32.dll` and `Ws2_32.dll` being imported means that functions related to accessing the internet is used.

### How

I will be using `DetectItEasy`. 

Assuming you have loaded your file, click on `import` 

![](assets/posts/MA/chap1/img5_DIE4.png)


This shows the imported `dll` as well as the functions from it


## 5 - Exported functions
Viewing the exported functions provides hints about what other executables that uses it does.

### How


I will be using `DetectItEasy` and `Lab01-01.dll`

Assuming you have loaded your file, click on `export` 

![](assets/posts/MA/chap1/img6_DIE5.png)

## 6 - PE file header - sections, IMAGE_FILE_HEADER , IMAGE_OPTIONAL
Viewing the virtual and raw size of the sections is one of the clues to determine if an executable has been packed. If the virtual size (size on disk) is significantly smaller than that of its raw (actual size when running) size, this means it has been packed. The main sections being `.text` which stores the code, `.rdata` which contains the import and export information (as seen above), `.data` which contains the global data such as global variables and `rsrc` which contains the resources used by the file such as the image for its icon

IMAGE_FILE_HEADER tells us when the executable had been compiled and more. However, these values can be altered after compilation

IMAGE_OPTIONAL's subsystem tells us if the executable has a GUI or it runs on the command line.


### How

Click on the arrow under `Sections` to open up a window showing more information about the size of the sections:

![](assets/posts/MA/chap1/img7_DIE6.png)

![](assets/posts/MA/chap1/img8_DIE7.png)



You can  view `IMAGE_FILE_HEADER` information by clicking on it:

(assets/posts/MA/chap1/img8_DIE_imgfileheader.png)

You can view `IMAGE_OPTIONAL`'s information by clicking on it

![](assets/posts/MA/chap1/img9_Subsystem.png)


## 7 - Obfuscation and packing
Obfuscation is the act of trying to hide something with that being the execution of malware in this case. Packing on the other hand is the compression of the original file. This makes basic static malware analysis way more chalenging.

### How to detect

#### Section names
Section names may be altered if a packer was used. One popular example will be the packer PEiD. This is `Lab01-02.exe` 's sections. This clearly indicates that UPX was used to pack this. However it might not always be so simple

![](assets/posts/MA/chap1/img10_sections.png)


#### Section virtual and raw sizes
If the virtual size differs greatly from the raw size , this suggest that packing might had been used.
#### Low import count 
If there is close to no functions being imported, that means it has been packed. `Lab01-03.exe` was used for this.

![](assets/posts/MA/chap1/img11_import.png)


