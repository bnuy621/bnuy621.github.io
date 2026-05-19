---
title: ASM - Memory Segments
date: 2026-04-25 00:00:00 +0800
categories: [ASM]
tags: [non technical]     # TAG names should always be lowercase
description: ASM
toc: true
---

## Introduction
<https://www.tutorialspoint.com/assembly_programming/assembly_memory_segments.htm>

## Segments
An assembly program can be split into memory segments which consists of the `data` segment where our variables are, the `code` segment
where our code is stored and the stack where local variables are temporarily stored. 

This means that if i replaced the keyword `section` with `segment` it will still compile. 

This is the contents of the assembly file:
```
segment.bss   ;allocate memory for variables that are yet to be initialised

segment.data  ;define variables and initialise them


segment.text
  global _start ;this tells the kernel to run from _start

_start:
```

The screenshot below shows that it is still able to compile:


![](assets/posts/ASM/post2/clip.png)


## References
<https://www.tutorialspoint.com/assembly_programming/assembly_memory_segments.htm>
