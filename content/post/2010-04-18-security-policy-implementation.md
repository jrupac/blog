+++
title = "Security Policy Implementation"
date = "2010-04-18"
tags = ["C", "cybersecurity", "programming"]
+++

This weekend has been taken up almost exclusively by coding (yet again :)). This
time, I'm working on a project for ELE 386, also known as Cybersecurity.
The task is to develop a working implementation of a well-known security policy
in a way that can be used to update permissions and check permissions. I am
implementing the Clark-Wilson model and building a multi-platform, command-line
interface. Considering I started this about 36 hours ago, I think I've
made pretty good progress. Normally, I'd write something like this in
Java, just due to the ease of using built-in data structures (TreeMaps,
HashSets, etc.) but the assignment kinda leaned towards C/C++ (yes, I know I can
write my own, but I don't have the time or patience to..). Seeing as
I've just picked up C this semester, I decided to go with that as a
challenge.

Yesterday, I hacked together the basics of control flow and some I/O. The
challenge here is that we are to _assume_ that the user will be malicious; in
fact, we will be trying to break each other's implementations in the next part
of the assignment.  So, every single input has to be extremely meticulously
validated before anything happens. In fact, I've completely decided against
using `scanf()` or even `fgets()` for user-related I/O; I need finer control.
So, I've decided to literally `getchar()` each character by hand, doing multiple
checks on bounds and legality of inputs. Every input has a pre-defined upper
bound and so far, it seems to be watertight. One problem that I encountered was
figuring out how to get rid of extraneous characters in the stdin buffer (for
example, if the user is asked for at most 3 characters and replies with 46
characters).  I tried a lot of different things and ended up with this approach,
a slight adaptation from [here][1]:

```c
void flushInput()
{
   char c = ' ';
   ungetc(c, stdin);
   scanf("%c%*[^n]%*c", &c);
}
```

Essentially, this little function first creates an arbitrary characters (a space
in this example), uses `ungetc()` to push the space onto the stdin buffer. Then,
using a bit of formatted string ninja, `scanf()` reads in the space, skips
everything that's not a newline characters, and then skips the character right
after that (the newline itself). I know it's not the prettiest piece of code,
but it's faster than doing `getchar()` in a loop and checking for `EOF`.

Once I had crossed that hurdle, I was able to quickly get through a lot of other
small issues. One source file quickly become 3 which quickly became like 9. The
layout and refactoring that I started doing early really helps to keep it all
sane. Last night, I also finally was able to glue in an MD5 hashsum
implementation with my code, as I only want to be storing the hashes of the
passwords instead of the passwords themselves. That ended up working pretty
nicely and made my implementation stronger.

This morning I was able to write the database importer that loads the file into
memory. I ended up going with an array of struct pointers, which in turn hold a
linked list of struct pointers as my primary data structure. So far it's working
well and I'll have to serialize this abstraction and write the exporter of the
database back onto disk. Should be fun. In the meantime, I've gotta write some
more code to validate queries, which can be rather long and complex. Did I
mention I need to get this working by Tuesday? It's gonna be a long night..

**EDIT**: One last note. Found an error in the [Bryant & O'Hallaron][2] book on
page 611. The function prototype for `execve()` is listed as:

```c
#include <unistd.h>

int execve(char *filename, char *argv[], char *envp);
```

However, the correct prototype is actually:

```c
#include <unistd.h>

/* Note the braces on envp */
int execve(char *filename, char *argv[], char *envp[]);
```

Not a huge deal, but it's an important function, so just wanted to point that
one out.

 [1]: http://www.velocityreviews.com/forums/t318260-peek-at-stdin-flush-stdin.html
 [2]: http://www.amazon.com/Computer-Systems-Programmers-Perspective-Introduction/dp/0582832411/ref=sr_1_2?ie=UTF8&s=books&qid=1271631629&sr=1-2
