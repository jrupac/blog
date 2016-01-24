+++
title = "nVidia Drivers and Ubuntu 10.04 Lucid"
date = "2010-05-14"
tags = ["drivers", "kernel", "linux", "programming"]
+++

I just completed finally correcting the mess that my upgrade to Lucid from
Jaunty left. Essentially, I didn't notice any issues for a few weeks but
then a recent update resulted in my nVidia drivers failing to load and me being
left in "low graphics" mode. I spent the past few days trying to
figure out what the hell actually caused my _graphics card driver_ of all things
to fail randomly. On my laptop, I have a nVidia Quadro NVS 160 card and the
laptop itself is pretty new. Also, considering my netbook had no issues with any
recent updates, I was further puzzled. I tried uninstalling and reinstalling the
drivers, trying legacy drivers, even attempting to re-compile the drivers myself
after downloading the kernel source (after leaving it on for about 15 minutes
and have both proc cores report temperatures of around 89 deg C, I decided to
stop it..).

Then I realized something. The problem had nothing to do with nVidia at all. In
fact it had nothing to do with anything graphically related either. The problem
stemmed from my actual upgrade to Lucid several weeks ago. What happened is that
during the upgrade process, I was asked if I would like to keep my current
`menu.lst` file. Since Lucid uses GRUB2 and no longer actually uses the
`menu.lst` file, and considering I had modified that file quite long ago, I
decided to keep it. The installation went on smoothly and I didn't notice any
major errors. The only very strange message I noticed was that every time I
booted into Ubuntu, I got a message like the following:

```
mount: mounting none on /dev failed No such device
```

In my actual boot sequence, I got some numbers after `/dev` and I couldn't
figure out where this error was coming from.  It didn't show up in any log
files either! Finally, I realized that running `uname -r` returned
2.6.31-14-generic - which was completely wrong considering the kernel was
updated in the latest release! Then, I realized that the kernel I was booting
into was not actually the latest kernel at all! From this I realized that I
needed to modify my `menu.lst` file manually and then update grub manually in
order for it to work. 

Before running the chance of completely destroying my machine, I decided to do
it ad-hoc, during the boot sequence itself. That way, the actual file
wouldn't be modified/destroyed/corrupted. So, I restarted and at GRUB, i
hit `e` and manually changed the numbers to reflect the latest
kernel. Success! It booted but my drivers still weren't working. So, with
this confidence, I went back to `menu.lst` and modified the kernel numbers.
Another reboot resulted in a successful load and no mount errors! 

At this stage, I knew that the drivers issue had to be related. So, I went to
System->Administration->Hardware Drivers and saw that I had a 173 version (my
attempt at a legacy installation) and the nvidia-current drivers, with the
nvidia-current driver "activated". Clearly, this was a lie, considering I was
running on vesa drivers! And not even trying the nouveau drivers made a
difference either. So, I figured that I would save myself the possibility of
screwing up the re-installation and let Hardware Drivers do it for me. I
"removed" the drivers and then "activated" them again. Then, I rebooted. 

To my surprise, everything worked! No errors on boot, a clean shutdown too! And,
my system load monitors displayed my GPU temperature too! Checking
nvidia-settings ensured that the true proprietary drivers were loaded. Finally,
I wanted to check if the GLX module had successfully loaded, so I ran `compiz
--replace` and even that worked without a hitch. Awesome. So it seems like I got
this thing to work finally. The final solution was a lot simpler than most
approaches. But what this does show is that there are serious issues with
"custom" upgrades - the upgrade successfully and silently completed and gave no
indication at all that there were any issues! I'll be submitting a bug report
when I get a chance.  I hope other people can read this and save themselves the
hours I wasted looking in all the obvious places for the error.
