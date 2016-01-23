+++
title = "Programming Puzzle 1 Solution"
date = "2010-12-11"
tags = ["C", "programming", "obfuscation"]
+++

This is solution to the previous blog post.

So there's a couple things going on here. They include trigraphs, strange
pointer arithmetic, casting, single and multi-line comments, and general
compiler abuse. Let's take this apart step by step:

## Trigraphs

Trigraphs were used several decades ago when keyboards then didn't have
all of the characters that we now have, such as curly braces, backslashes, etc.
So, in order to encode these, three-character strings were used. Typically, the
original trigraphs of C at least had a `??` prefix. (In C99, the
notion of a standard prefix for trigraphs was removed). The ones I used in this
program were `??/` (backslash), `??)` (left square brace), `??(` (right square
brace), `??!` (pipe), `??<` (right curly brace), and `??<` (left curly brace).

The first one is the most interesting. In C, the backslash character can be used
to write strings that are too long to fit on a line (the only thing to note is
that the continuation of the string must occur _immediately_ on the next line,
without spacing. Since this breaks code indentation, other techniques are
generally used (such as string concatenation either through `strcpy`, `strncpy`,
`memcpy`, or less traditionally, two literal strings right next to each other.
(There are other ways too). However, since trigraph (and digraph) substitution
is done in the pre-processing stage, this concept of breaking onto new lines
works for anything! So, let's take the original source code given and
substitute all of our trigraphs in and fix indentation:

**Caveat:** GCC4 turns off trigraphs by default as they make reading code much
more complicated if used obscurely. That's the reason behind the "w/
appropriate flags" condition on the problem statement. To turn it back on,
compile with `-trigraphs`.

```c
#define define include
#define include define
#include <stdio.h>
#include <stdlib.h>
main(/*int, char*/) { char* c;c = calloc(10, sizeof(*c))//c = "hello, world" ; #define int char ;int
x=(sizeof(int)<<sizeof(c))+(sizeof(int)>>sizeof(*c)), i=* c;do
c[++i]="0123456789abcdef"[x & 0xf];while(x>>=sizeof(i));
for(;x|i;--i)putchar(c[i]); }
```

That cleans up a lot of things. Now we see that there are actually comments in
this code! In fact, I just threw several clearly visible phrases (like the
string "hello, world" and `main()` arguments) to trick a casual viewer into
thinking that the functionality of the program was different from what it
actually did.

## Macro Definitions

Interestingly, the first two lines do nothing at all. To realize why, note that
the pre-processor only traverses the source once. So, if we had actually made
changes, they would not be reflected fast enough (on the first pass) to be
properly dealt with by the pre-processor. The compiler would then complain about
these unknown words (if the pre-processor didn't fail first, which it
almost certainly would). So we can just remove those lines. Below is the next
revision of the source code, with comments and dead code removed and further
indentation fixing.

```c
#include <stdio.h>
#include <stdlib.h>
main() {
  char* c;
  c = calloc(10, sizeof(*c));
  int x=(sizeof(int)<<sizeof(c))+(sizeof(int)>>sizeof(*c)), i=* c;

  do c[++i]="0123456789abcdef"[x & 0xf];
  while(x>>=sizeof(i));

  for(;x|i;--i) putchar(c[i]);
}
```

Now this is looking much simpler. Let's see if we can clean this up even
more and then analyze the essential core of the program.

## Sizeof Operator

This is a very simple part. The need for me to specify is a 32-bit OS is clear
here: the size of pointers will be either 4-bytes or 8-bytes based on 32-bit or
64-bit operating systems. Now, I was sloppy in this program for assuming the
size of ints and chars to be 4 bytes and 1 byte, respectively, but I don't
feel too bad since this was just a small puzzle, I specified the compiler and
language specification, and the functionality of the program does not depend on
those values except in one place..

Let's substitute the numeric constants too and see where we arrive:

```c
#include <stdio.h>
#include <stdlib.h>
main() {
  char* c;
  c = calloc(10, 1);
  int x=(4<<4)+(4>>1), i=* c;

  do c[++i]="0123456789abcdef"[x & 0xf]; while(x>>=4);

  for(;x|i;--i) putchar(c[i]); }
```

Now we're at the last step.

## Pointer Arithmetic and Bit-Shifting

Now by using a piece of paper or a programming calculator, we can trivially see
that `4<< 4` is 64 (`0b100` => `0b1000000`) and  `4>>1` is 2 (`0b100` => `0b10`)
to give `int x = 66`. Nice. Now on line 11, we are setting the value of `x` to
be value of `x` right-shifted by 4 bits. The reason for this will become
apparent in the following paragraphs. Finally, if we follow the logic of the
program, we see that in the do-while loop, we are setting the number to be a
number definitely smaller than itself each time. The exit condition for the loop
is when the clause is false. In C, this is a zero. So, we see that the loop will
break when `x = 0`. So what's with that last for-loop? We are doing a
binary OR with a zero value. It therefore has no effect and we are just checking
if `i` is 0.

Now let's deal with the main part of the program. What the hell is line 10
doing? This is where understanding the meaning behind the []-operator plays a
role. To put it simply. this is often called "syntactic sugar" since
it's just a way to write an otherwise slightly more complicated
expression. It's important to realize therefore that `a[i] == i[a]` since
both are just equal to `*(a + i)`, in pointer arithmetic. However, indexing also
works for _literals_ in C. So, realizing that the expression `x & 0xf` masks off
the last four bits of `x`, we now understand that all this is doing is setting
the next character of the `c` array to the hexadecimal representation of the
trailing nibble (four bits) of `x`. Thus, noting that we are actually printing
the array backwards at the end since the representation is backwards, we see
that all we're doing in this program is printing out the hexadecimal
representation of decimal 66, which is conveniently 42. Note that there is not
newline character printed after this.

## Conclusion

If you're paying attention at this point, you'd realize that
there's no return type listed in the function header of `main()` nor is
any value actually returned. Wait but isn't main supposed to return 0?
Well, the usual approach is to return 0 on success, although any number can
returned on error. This is where we have abused our compiler. All functions with
non-listed return types in the function header have return type `int` and if no
number is returned at the end of `main()`, we automatically return 0. So
everything is taken care of for us nicely.

This concludes the solution of the first programming puzzle. Maybe I'll
put up more of them in the future.

