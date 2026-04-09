---
title: "Practical Malware analysis: arithmetics"
date: 2026-04-01 00:00:00 +0800
categories: [ASM, malware analysis]
tags: [basics]    
description: binary available at 
toc: true
---

## Introduction 
Comparison of code in C vs how in looks in Assembly. Im using Linux with gcc compiler so it might look a bit different on your side.


## Original code

```bash
#include <stdio.h>

int main(void) {
    int a = 0; 

    printf("a is %d\n", a);

    a = a + 11;
    printf("a is %d\n", a);

    a = a - 1;
    printf("a is %d\n", a);

    a--;
    printf("a is %d\n", a);

    a++;
    printf("a is %d\n", a);

    a = a * 2;
    printf("a is %d\n", a);

    a = a % 3;
    printf("a is %d\n", a);

    return 0;
}

```
## Addition

### C

```yaml
a = a + 11;
printf("a is %d\n", a);
```

### Assembly
![](assets/posts/MA/chap6/post6/add.png)

## Subtraction

### C

```yaml
    a = a + 11;
    printf("a is %d\n", a);
```

### Assembly
![](assets/posts/MA/chap6/post6/sub.png)


## Decrement

### C
```yaml
    a--;
    printf("a is %d\n", a);
```

### Assembly
![](assets/posts/MA/chap6/post6/dec.png)


## Increment

### C
```yaml
    a++;
    printf("a is %d\n", a);
```
### Assembly
![](assets/posts/MA/chap6/post6/inc.png)


## Mulitplication
### C
```yaml
    a*2;
    printf("a is %d\n", a);
```
### Assembly
![](assets/posts/MA/chap6/post6/mul2.png)


We can see here that `shl` was used to shift it one bit to the left. This is a trick used to make multiplication faster.
For example:

```yaml
mov eax, 5      ; EAX = 5 (binary 00000101)
shl eax, 1      ; EAX = 10 (binary 00001010) - Multiplied by 2
shl eax, 2      ; EAX = 40 (binary 00101000) - Multiplied by 4 (total shift of 3 bits from original)   
```

## Modulo

### C
```yaml
    a = a % 3;
    printf("a is %d\n", a);
```

### Assembly
![](assets/posts/MA/chap6/post6/mod.png)

    
We can see here that lots of optimisations was used such as multiplying by a certain number in this `55555556h` and using `shr` .