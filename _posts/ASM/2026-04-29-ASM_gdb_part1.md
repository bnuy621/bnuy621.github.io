---
title: ASM - gdb debugging
date: 2026-04-29 00:00:00 +0800
categories: [ASM]
tags:      # TAG names should always be lowercase
description: ASM
toc: true
---

## Introduction
This is a short guide to learning how to use gdb to debug your assembly program. I'll update this when i find new stuff.

## compiling and linking your assembly program
Firstly, run this command to create the object file:

`nasm -f elf64 -g -F dwarf -o [file_name].o [file_name].asm`

Now link it with this command:

`ld -o [file_name] [file_name].o`

## Debugging 
Run this command to debug your assembly program with gdb:

`gdb [file_name]`



## Note
Please use a null terminator for each variable if not commands like 'x' will display both the input variable and the rest till it caps out.


I will show some of the commonly used commands:

## Displaying assembly code - list [filename]
To view the first 10 lines of the source code of the assembly program, run the following command:

```
list
```

![](assets/posts/ASM/post6/list.png)

To view the next 10 lines, run it again.

![](assets/posts/ASM/post6/list2.png)

If you don't want to run `list` multiple times, set the number of line shown via `set listsize [no. of lines]`. Now instead of running `list`, run `list.`

## Display variable's data
Use the following commands to display a variable's data:

```
x [variable_name]
```

or

```
p ([type]) [variable_name]
```

![](assets/posts/ASM/post6/p1.png)


## Breakpoints
Breakpoints are very important when debugging, they allow you to 'pause' your code. 

### Setting a breakpoint 
To set a breakpoint run the following command:

```
b [line_number]
```
![](assets/posts/ASM/post6/b_set.png)

### Viewing breakpoints
To view all currently set breakpoints, run the following command:

```
info b 
```
![](assets/posts/ASM/post6/b_info.png)

### Deleting breakpoints
To delete a breakpoint, run the following command:

```
del breakpoint <breakpoint_number>
```
![](assets/posts/ASM/post6/b_del.png)

## Viewing register data
Run the following command to display the values in all of the registers:

```
info all-registers
```

You must first run the program and execute at least one line of instruction if not there will be no values in the registers.


