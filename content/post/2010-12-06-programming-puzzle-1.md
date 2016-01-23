+++
title = "Programming Puzzle 1"
date = "2010-12-06"
tags = ["C", "obfuscation", "programming"]
+++

Consider the following program written in pure C89 on a 32-bit OS:

<!--more-->

```c
#define define include
#define include define
#include <stdio.h>
#include <stdlib.h>
main(/??/
*int, char*??/
/)??<char* c;c = call??/
oc(10, sizeof(*c))//??/
c = "hello, world" ; #define int char
;int x=(sizeof(int)<<sizeof(c))+(sizeof(int)>>sizeof(*c)), i=??/
* c;do c??(++i??)="0123456789??/
abcdef"??(x & 0xf??);while(x>>=sizeof(i));for(;x??!i;--i)putchar(c??(i??));??>
```

Two questions (solve without computer):

  1. Does it compile (w/ appropriate flags)? [Tested on GCC 4]
  2. What is the output?

Just a few tricks, but shouldn't be too hard to solve. I'll give the answer in
the coming days.
