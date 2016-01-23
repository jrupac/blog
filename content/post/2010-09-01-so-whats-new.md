+++
title = "So What’s New?"
date = "2010-09-02"
tags = ["android", "computer science", "crowdsourcing", "android",
"programming", "research", "google"]
+++

It's been a few weeks since I last updated this with a real post, so here
it is.

I finished my research for the summer a few weeks back. I'm probably going
to continue it again once the school year starts, but I decided I wanted at
least some time to work on different things during the summer as well as catch a
break. At the time that I ended, most of the major tasks assigned to me had been
completed. That is, all but one program module had be written and polished and
most of it had been used to generate at least first-run data. I can't go
very much into the details about what the modules did exactly, but the broad
concepts are the same: write software that parses the millions of lines of log
data efficiently and offers a way to gain greater insight into it

Despite the fact that some of this code will only be run once, I decided to
nevertheless invest the time into making it nice and modular, etc. This is a
time when I truly saw the importance of good code design. Code that I had
written at the beginning of my research period, about two months back, really
does look alien if you've lost touch with it. Among the people I was
working with, I started first, so there was no code to start off from. I ended
up making up my own standards for how to run certain modules - a certain
combination of command-line flags and `stdin`/`stdout` redirects and all. In
this process, of course a lot changed and later parameter paradigms greatly
differed from the early ones. At some point, I realized that it was a major pain
to remember how to run each program so rewrote lots of my previous code to
accommodate for that. For example, there was a certain little script that I used
to run the same program on many log files over a period of time. This little
script started off first as an inline bit of Bash fu that I could start in the
background and let it run through the day and into the night if necessary. It
then evolved into a script of its own, became more standardized to accept the
executable and as my knowledge of Bash improved, was written more efficiently.
In fact, it even went on to become so commonly used that I included it in nearly
module I wrote. The same goes for implementations of commonly used data
structures - these were soon put in a common `lib/` folder and linked to
from various modules. Even commonly used functions that mapped to all elements
of those data structures were placed in a specials `utils.c` file. Another
useful little macro directive that I used to help with (possibly careless,
irresponsible, and brash) memory management was a wrapper on GNU free that was
double-free safe. So, every single `free()` call was replaced with `nxfree()`
with the following implementation:

```cpp
#include <stdlib.h>

/*
 * Convenient wrapper for nfree().
 */
#define nxfree(s) nfree((void**)&amp;s)

/*
 * Wrapper for GNU free. Checks for NULL and sets pointer to NULL
 * after freeing. Avoids accidental double frees.
 */
#define nfree(p) {if(*(p)) free(*(p)); *(p) = NULL;}
```

So, pretty simple, but useful. I ended up using many such little snippets of
code to make my life easier. In addition, I also rewrote my original `Makefiles`
to be more modular. I ended up creating a standard template for all `Makefiles`,
with only trivial changes to customize it for the module in question. The
template is generic enough for use pretty much anywhere `gcc` works and is
below:

```
CC = gcc CFLAGS = -Wall
CDBGFLAGS = -Wall -g LIBS =

LIBOBJS = ../../lib/*.o   # Shared Library Code Path

TARGET = <Release Target Name>
DBGTARGET = <Debug Target Name>

OBJS = <All Source Files Names, with .o Extensions>
DBGOBJS = <All Source Files Names, with Dbg.o Extensions>

$(TARGET) : $(LIBOBJS) $(OBJS) $(CC) -o $@ $^ $(CFLAGS) $(LIBS)

$(DBGTARGET) : $(LIBOBJS) $(DBGOBJS) $(CC) -o $@ $^ $(CDBGFLAGS) $(LIBS)

%.o : %.c $(CC) -c -o $@ $< $(CFLAGS)

%Dbg.o : %.c $(CC) -c -o $@ $< $(CDBGFLAGS)

all: $(TARGET)

debug : $(DBGTARGET)

clean: rm -f $(TARGET) $(DBGTARGET) $(OBJS) $(DBGOBJS)

clobber: make clean && rm -f *~
```

Now after research was done, I got a bit more time to work on the Mozilla
Crowdsourcing project I've mentioned before. We completed Phase 1 a few
weeks back (check the Mozilla Labs' blog for all sub-teams findings and
[here][1] for just my sub-team's). After that, we got started on Phase 2,
which consists of designing a Design Challenge using our findings that took into
account the various things we picked up along the way. The final result of the
Crowdsourcing team's designs will eventually replace the current model of
the Lab's Design Challenges. Last Sunday was the deadline for that and our
sub-team had a pretty concrete set of guidelines. These will soon be written up
and fleshed out into a blog post. Once this stage is complete, we will take our
three sub-teams' designs and combine them into one, which will be our
official proposal. Then come the actual implementation of the proposal, which
may include some actual development and/or extension of existing software.
I'll speak more of this as it comes up in the next few weeks.

Apart from that, I'm also just doing my usual things, such as testings and
bug reporting for Minefield (Firefox Panorama officially ROCKS) and Chrome. I
also decided on one Saturday to re-root my Droid from scratch since a couple
things needed fixing. I was unable to install updates for certain applications
such as Google Maps, doubleTwist, WordPress, and Facebook for Android. And
Chrome-to-Phone was not working correctly either. In addition to the fact that
my phone was still feeling rather sluggish. So, after many hours of figuring out
what the hell was going on, I decided to format everything, install ClockworkMod
Recovery, and flash a custom rooted Froyo ROM. This was actually the FRG01B
build of Froyo, which replaced the earlier update that I had previously
installed on my Droid. I also got SetCPU (I'm an xda-developers member, so
I got the APK from there &#8211; if I continue using it, I'm going to
consider donating to the developer). So far, my experience with that has been
pretty much flawless. I set up a few failsafe profiles just to prevent
spontaneous combustion of my device and the homescreen widget works great. I
used a 1GHz kernel again and am currently running at a max of 1000MHz and a min
of 500MHz on &#8220;performance&#8221; mode. My CPU temperature usually hovers
around 33 to 35 degrees C, which is pretty normal and the phone is stable.
Obviously, it feels a lot more snappy with the overclocked kernel and runs
pretty smoothly on most things I've thrown at it. I'm pretty happy
with that. There actually is another minor upgrade that was released after
FRG01B that basically just enabled the download of the official Adobe Flash
10.1. Except that that build hasn't been rooted yet and I side-loaded
official 10.1 APK anyway, so I chose not to upgrade for now at least. From all
accounts, the newer version is the last of the Froyo upgrade process for Droid
and does NOT add any new features.

Well that's what's keeping me busy right now. I've got about
two weeks before college starts again, so I'm prepping myself as necessary
for that. I expect this year to be pretty challenging again, but at least I know
I will enjoy the classes even more now, having had more flexibility in
choosing them. In the meantime, I think I'll finally take a look at
[Google Code University][2] and see what they offer &#8211; been wanting to do
that for quite a while.

 [1]: http://mozillalabs.com/conceptseries/2010/08/17/crowdsourcing-project-summary-of-crowdsourcing-literature/
 [2]: http://code.google.com/edu/
