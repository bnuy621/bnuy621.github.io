---
title: "Practical Malware analysis: Global vs Local variables"
date: 2026-04-01 00:00:00 +0800
categories: [ASM, malware analysis]
tags: [basics]    
description: binary available at 
toc: true
---

## Introduction
We will be observing the difference between global and local variables in C and in Assembly. We will be observing two seperate binaries for this.

## Global

### C

```bash
#include <stdio.h>
int x = 1; 
int y = 2; 

int main(void)
{
    x = x + y;
    printf("total = %d\n", x);
    return 0;
}
```

### Assembly
![](assets/posts/MA/post8/global_assembly.png)


### Explanation

We can see here that the global variables X and Y are assigned values before `__main()` was called:
![](assets/posts/MA/post8/global_pt1.png)

This is where the `x = x+y` takes place. Firstly, the values of `x` and `y` are loaded into `edx` and `eax` respectively. This is followed by `add eax, edx` AKA `y+x`. Lastly, this value is moved to the specialised register holding the value of `x`. This causes the value of `x` to be that of `x+y`.

![](assets/posts/MA/post8/global_pt2.png)


Afterwards this value is displayed as inferred from the `printf` call.

## Local

### C

```bash
#include <stdio.h>
int main(void)
{   
    int x = 1; 
    int y = 2; 
    x = x + y;
    printf("total = %d\n", x);
    return 0;
}
```
### Assembly
![](assets/posts/MA/post8/local_assembly.png)

### Explanation
We can see here that the local variables `x` and `y` are initialised with a negative offset:

![](assets/posts/MA/post8/local_pt1.png)


After `__main` was called, the values `1` and `2` were stored into X and Y:

![](assets/posts/MA/post8/local_pt2.png)

The values of `x` and `y` were added with the result being stored to `x`. Hence, `x=x+y`:

![](assets/posts/MA/post8/local_pt3.png)


