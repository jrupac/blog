+++
title = "Rooted Droid + Android 2.2"
date = "2010-07-15"
tags = ["android", "froyo", "motorola"]
+++

So I finally took the leap and rooted my Motorola Droid!

It was a completely painless procedure. My main goal was in order to get full
access to my own phone (which I think should be a given, since the device is
mine..). Then, I got a little zealous and decided to take it a bit further. I
figured hell, why not see what flashing Froyo will do? So I did. And that was
painless too. The main thing that was stopping me from doing this a long while
ago was because last I had heard that WiFi was not working. This was a
showstopper for me because since I do not have a Verizon subscription, I only
use my Droid over WiFi. However, after looking at a tutorial provided by the
friendly guys at **[The Unlockr][1]**, I was up in no time.

For the record, the exact tutorial that I used was [this one][2]. I was
originally going to write up my own set of instructions giving my personal
experience, but the tutorial was so clearly written that I literally just
followed it exactly and everything worked nicely. A couple of notes to point
out:

  * Be patient. Pretty much the only way you can mess this is up is if you jump
    past important instructions, stop without completing the procedure, or get
    tired of waiting for something to load and turn off your phone/pull out the
    battery.
  * Rooting will destroy all your data/apps on the phone, although files on your
    sdcard will remain, such as photos, music, etc.
  * I did not re-activate my phone after rooting, although it will ask you to.
    You can bypass that if you want by looking below how to skip the tutorial,
    (or by hitting the Home button if that works).
  * I got one message when I was rooting the phone (the first part) saying that
    a certain file did not exist on my sdcard (I forget the exact name for it
    now, sorry!). I safely ignored the message and continued and there was no
    issue.
  * I got a few more warning messages during the flashing of the new kernel.
    These too I was able to safely ignore.
  * I used the 1GHz kernel, so technically I overclocked my phone a bit. Since
    this is not my primary device, battery life is not too much of an issue when
    compared to performance, so I decided to go with it. I'm not badass
    enough to try the 1.25GHz kernel; I'll leave that to someone handling
    their phone with flame-retardant gloves.
  * Even with the 1GHz kernel, the phone is pretty stable so far and I
    haven't had any issues (only had it for an hour or so though, so
    remember that).
  * When the phone first reboots after flashing Froyo, it takes all eternity.
  * In order to skip past the Android tutorial, touch the four corners of the
    screen clockwise, starting with the top-left. This is a cool tip I picked up
    from a couple videos on YouTube.
  * After the first load, Android is _very_ slow. This confused me for a while
    until I realized that it takes a bit of time for the JIT cache to warm up.
    Once the caches were hot, performance greatly increased. This took about
    5-10 minutes for me. Menus went from stuttering to buttery smooth. Sweet!
  * I downloaded Adobe Flash: it eats two batteries for breakfast, but works
    pretty nicely (for a phone).  It's actually pretty cool to see Flash
    video load in-page. But for your sake, please set Plugins to load On-Demand
    or your phone will suck at battery life.
  * I was able to get Flash to work on the NYTimes homepage and it loaded fairly
    well. Audio was less than perfect and audio was a bit jaggy, but again,
    it's better than nothing.
  * Droid has five home screens! Cool. Not sure if this is just something about
    my custom ROM or if the official version will have it as well. The other
    advertised aspects of Froyo including improved universal search, new dock
    bar, 3D application drawer, etc. are all there too.
  * You still have root access after flashing 2.2.

And so yup, that's an overview of my thoughts on it. It was really quite
painless. The longest step was waiting for Droid 2.o.1 firmware to download
(it's like 130MB) over Megaupload speeds. Remember, you have to downgrade
to 2.0.1 and then root and then go to 2.2. The process was made incredibly
straightforward and simple, so I need to thank all of the dozens of devs and
modders from all over the Internet including the Android Forums, xda-developers
forum, and The Unlockr. You guys really make my day. Thanks!

 [1]: http://theunlockr.com
 [2]: http://theunlockr.com/2010/06/06/how-to-get-android-2-2-froyo-on-your-motorola-droid-right-now/
