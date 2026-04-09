---
title: "Practical Malware analysis: nested if"
date: 2026-04-01 00:00:00 +0800
categories: [ASM, malware analysis]
tags: [basics]    
description: binary available at 
toc: true
---

## Introduction
Comparison of code in C vs how in looks in Assembly. Im using Linux with gcc compiler so it might look a bit different on your side.

## Original code

### C
```bash
#include <stdio.h>

int main(void) {
    int x = 0;
    int y = 1;
    int z = 2;

    if (x == y) {
        if (z == 0) {
            printf("z is zero and x = y.\n");
        } else {
            printf("z is non-zero and x = y.\n");
        }
    } else {
        if (z == 0) {
            printf("z is zero and x != y.\n");
        } else {
            printf("z is non-zero and x != y.\n");
        }
    }

    return 0;
}
```

### Assembly

![](assets/posts/MA/chap6/post10/assembly.png)

## Explanantion

Firstly, as `x` , `y` and `z` are local variables they are initialised at a negative offset. They are assigned the values `0`, `1` and `2` respectively.

![](assets/posts/MA/chap6/post10/pt1.png)

Secondly, the value of `x` is compared to `y` to find out if they are the same. If they are, the code takes the `red` path. If they are NOT, the code takes the `green` path.

![](assets/posts/MA/chap6/post10/pt2.png)


If `x` is equal to `y` and `z` is equal to `0`, the message `z is zero and x = y.` will be displayed:

![](assets/posts/MA/chap6/post10/pt3.png)

If `x` is equal to `y` and `z` is NOT equal to `0`, the message `z is non-zero and x = y.` will be displayed:

![](assets/posts/MA/chap6/post10/pt4.png)

If `x` is NOT equal to `y` and `z` is equal to `0`, the message `z is zero and x != y.` will be displayed:

![](assets/posts/MA/chap6/post10/pt5.png)

If `x` is NOT equal to `y` and `z` is NOT equal to `0`, the message `z is non-zero and x != y.` will be displayed:

![](assets/posts/MA/chap6/post10/pt6.png)
