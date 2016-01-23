+++
title = "Jolicloud 1.0 + Dell Mini 10v"
date = "2010-07-31"
tags = ["dell mini", "HTML 5", "jolicloud", "netbook"]
+++

In my continuous pursuit of trying out cool, new software and hardware, I
recently decided to give Jolicloud a go. To be certain, this isn't
something that I just decided one day. In fact, I had seen and done a bit of
research into Jolicloud many months ago, long before any announcement of the 1.0
release. The one major factor that stopped me from pursuing it any further was
the actual fact that it was a cloud-based OS and therefore, I assumed that
without a persistent connection, I would helpless. This, however, it not
necessarily true with Jolicloud.

The device that I tried Jolicloud on (and am writing this post in) is a stock
Dell Mini 10v. It's your very typical netbook, with 1.0GB RAM, 160GB HDD,
Intel Atom N270 clocking at 1.60GHz. Initially, I ordered it downgraded to
Windows XP. Soon after I got it, I upgraded it to Windows 7 Home Premium. And
about 6 hours after that, I partitioned the disk and installed Ubuntu Linux.

If you're keeping track, that now means that there are three OS
installations on this netbook. But that's not strictly accurate since
Jolicloud is currently installed on a virtual partition of the Windows partition
(think Wubi with Ubuntu). So, it shows up under the Windows chainloader and not
directly in the GRUB bootloader. Of course, once you actually boot into
Jolicloud, it's virtually impossible to tell.

So about three weeks ago, I finally took the leap and installed it. Right off
the bat, I felt familiar in the interface. Recall, this is was pre-1.0 release,
so the interface was completely different from the new 1.0 HTML5 interface. The
reason I felt familiar is because it was nearly identical to UNR (Ubuntu Netbook
Remix). This brings me to a good point: why did I even bother trying this OS?
Well, of course I am a huge fan of Ubuntu and I think it's pretty apparent
that it's one of the easiest Linux distros to use. It does work great on
my netbook in terms of performance, although not so much in terms of
_usability_, in the sense that it's painfully clear that stock Ubuntu is
not written with the netbook in mind. With Gnome-Do, it's a whole lot
easier to interact with the OS and so the need for a good universal launcher is
a must. Although the Mini 10v is an otherwise great netbook that has satisfied
all of my needs, the touchpad is absolutely horrendous in every conceivable way.
Touch-based left- and right-click buttons is a tremendous mistake. In theory it
sounds like a great idea, except that _even the portions of the touchpad where
you're supposed to push down to initialize a click are touch-sensitive._
Thus, unless you have a truly unwaivering hand (in which case you should just
use this skill alongside a magnetized needle to write all your code), you will
definitely move the cursor which clicking and thus so click accuracy sucks. This
was an important consideration since I want to use the touchpad as little as
possible. Jolicloud 1.0 makes that possible.

I decided to hold off on writing this post till I got chosen for the upgrade
since I didn't feel it was fair to make a judgement before seeing what the
latest version held. And I'm glad I did because the 1.0 release is in most
ways a huge step-up from before.

First of all, the well-known and talked-about HTML5 app launcher interface is
just great. It's much, much cleaner and easier to use than the previous
layout, which mimics UNR. Indeed, the line between file browser, app browser,
settings, and app "store" is very nicely and elegantly merged into
an intuitive interface. I would have liked mouseover descriptions of the icons
in the top menubar, but that's okay. Installing a new app (either an
actual desktop application or thin wrapper for a web-based client based on
application shortcuts available in Chromium) is quite literally a one-click
procedure. It's very plain and simple when an application is being
downloaded and even though some of the icons are not quite recognizable as to
their purpose, it all makes sense in context.

When you first boot into Jolicloud, it asks you to set up your wireless
connection yourself. Not all-together too shabby since the underlying OS does
pretty much all of the work for you, but it'd be nice to see that process
a bit better integrated into the starting up of the OS. Like having the list of
available networks clearly visible in the center of the screen instead of being
hidden away behind a mouseclick on the Wifi icon. Not a big deal though. If you
think Ubuntu is good at getting drivers and other configurations of hardware
done for you, you'll be really happy with Jolicloud too. I didn't
have to touch one thing and it in fact loaded up my Broadcom proprietary driver
automatically too (and informed me of so). Another interesting gnome-panel
applet-esque icon allows you to underclock your processor and set up power
plans. This is interesting but I haven't looked into it. Of course, the
underclocking is to save on battery life, but I haven't tested that out
(the Mini 10v with 3-cell battery has one of the worst battery lives I've
ever come across - about 2-3 hours, which really sucks for a netbook, so I
may actually look into this later on).

The apps I downloaded were still there when I completed the upgrade. The
Launcher, mapped to the Windows key, allows very quick access back to the home
screen from within any application. Hitting the Tab key brings focus to search
box, which allows you to search Google or your downloaded apps/friends/other
apps in the store. This is a really neat combo which I'll be using a whole
lot more. Overall, the interface is really great. Icons are large and crisp for
my standard 1024x600 display.

There's one thing in particular that truly annoys me though. And
that's the demotion of my most useful and favorite built-in applications
that were available before to a submenu "Legacy Apps". This includes
gnome-terminal and gedit. This makes me mad because outside of the popular
console-based text editors, I still find gedit a superior editor over the
others. And of course, how can I function without gnome-terminal? These apps are
also not indexed in the Launcher searchbar nor are they available in the app
store (neither is Google Chrome, as it was before, but Chromium is..). If I
could find some way to put this in my main app list, then I could be okay with
that.

I'll continue to give more of my impressions of Jolicloud and info about
my experiences with it as time goes on. All in all, I'm quite impressed
with where it now and it's a pleasure to use. If it continues to fair
well, I might end up using it a whole lot more with Windows 7 on my netbook.
