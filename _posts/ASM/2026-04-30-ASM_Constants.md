---
title: ASM - Constants
date: 2026-04-30 00:00:00 +0800
categories: [ASM]
tags:      # TAG names should always be lowercase
description: ASM
toc: true
---


## Introduction
Constants are similar to variables, this post will cover the various way to define a constant. 

## EQU
This is the same as `X = Y` in Python. The syntax for it in assembly:

```
CONSTANT_NAME EQU expression
```

For example:

```
BNUY EQU 621
```
### Example

```
section .data
  VAR_X EQU 5 ;beware of non-scalar
  VAR_Y EQU 10
  RESULT_Z EQU VAR_X * VAR_Y

section .text
  global  _start
_start:
  mov rax, RESULT_Z
  mov rax, 60
  syscall
```
When trying to view `RESULT_Z` 's value in gdb, i set a breakpoint at line 9 so i can view the register `RAX` which contains RESULT_Z.

![](assets/posts/ASM/post8/EQU.png)

![](assets/posts/ASM/post8/EQU2.png)


As when running the command `info  variables` none showed up as seen below:

![](assets/posts/ASM/post8/EQU3.png)

This is due to these labels/variables not being initialised in memory and only exist when its assembled. This means you can't really view it in memory via 'info variables'. This is why i retrieved its value via the register it was 'moved' to.

## %assign
This is similar to the `EQU` directive, but you can redefine the constant later on in the code. The syntax for it will be:

```
%assign BNUY 621
```

### Example

```
section .data
  %assign BNUY 621

section .text
  global  _start
_start:
  mov rax, BNUY
  %assign BNUY 620
  mov rax, BNUY
  mov rax, 60
  syscall
```

I added 2 breakpoints, one at line 8 and 10 to observe the changes in register `rax`:

![](assets/posts/ASM/post8/assign.png)
 
At the first breakpoint i ran `info all-registers`, this shows that `BNUY` had a value of `621`:

![](assets/posts/ASM/post8/assign2.png)

Upon inspection at the next breakpoint, we can see that the value of `rax` is now `620` as shown below:

![](assets/posts/ASM/post8/assign3.png)

This means that the value of `BNUY` changed to `620`

## define
This allows for both numeric and string constants. This is similar to the `#define` in c. Similarly to `%assign` you can redefine the constant later.

### Example 
```
section .data
  NUM_1 db "123456"
  STR_1 db "bnuy"
section .text
  global  _start
_start:
  %define TMP STR_1 
  mov rax, TMP
  %define TMP NUM_1
  mov rax, TMP
  mov rax, 60
  syscall
```
Firstly, i set up my breakpoints at line 9 and 11:

![](assets/posts/ASM/post8/define.png)


At the first breakpoint, the value of the register `RAX` is `4202502`:

![](assets/posts/ASM/post8/define2.png)


This is an address, to get the constant from this address we will use the following command:

```
x/s [memory_address]
```
This gives us `bnuy` as shown below which corresponds with the assembly code:


![](assets/posts/ASM/post8/define3.png)

At the second breakpoint, the value of the register `RAX` is `4202496`:


![](assets/posts/ASM/post8/define4.png)

Similarly, we will retrieve the constant at the address as shown below:

![](assets/posts/ASM/post8/define5.png)



As we did not put a terminator this caused it to also output `bnuy` which starts at `4202502`. But we can already guess that it is supposed to be `123456`. To output just `123456`. I will use the following command:

```
x/<number of bytes to output> [memory address]
```
![](assets/posts/ASM/post8/define6.png)


## References:

<https://www.tutorialspoint.com/assembly_programming/assembly_constants.htm>
<https://github.com/0xAX/asm/blob/master/content/asm_2.md>
