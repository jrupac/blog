+++
title = "Force Upgrade Verizon Galaxy Nexus to 4.0.4"
date = "2012-05-07"
tags = ["adb", "android", "galaxy nexus", "mempodroid", "verizon"]
+++

There's a new OTA update that's been dropping for some people on
Verizon, but not everybody. I didn't feel like waiting, so I decided I
wanted to do it manually. I have reposted the directions I found online to do
this process.

**NOTE: This is ONLY for Verizon LTE Samsung Galaxy Nexus phones running 4.0.2.
This will NOT work if you are running a previous build of 4.0.4. By doing this,
you accept all responsibility for anything that may happen to your phone.**

We will exploit a bug in the 4.0.2 kernel to temporarily give us root access.
Then, with root access, we will write the update file to the cache partition so
that it can be picked up by the stock recovery on boot and then used to flash an
upgrade. This will work for **unrooted phones with locked bootloaders**.

* First, create a new folder and download the **mempodroid** executible, which
  can be found [here](https://github.com/saurik/mempodroid). You can read about
  how the exploit works on there. A direct link to the file is
  [here](http://cache.saurik.com/android/armeabi/mempodroid).

* Next, download the **official update file** and rename it to **update.zip**.
  This used to be on the Google file cache for a bit, but it was taken down.
  There are many places you can download this and it doesn't matter where you
  get it from. The important part is that it has the MD5sum:
  **d8cddf4a824cb8beda1b03b8b5586da4**.

* Now it is time for us to push these files onto their correct places. This
  assumes that you have the **Android SDK** installed so that you can use ADB
  (Android Debugging Bridge) to push files.

* [You are in your computer's shell] Push the update to the root of the
  ```sdcard``` partition:

```bash
$ adb push update.zip /sdcard/update.zip
```

* Next, push the mempodroid executible to a temporary location:

```bash
$ adb push mempodroid /data/local/tmp
```

* Log in to the ADB shell to do further modifications.

```bash
$ adb shell
```

* [You are now in an ADB shell inside your phone] Move to the directory where
  the mempodroid file is and change its permissions so that we can execute it.

```bash
$ cd /data/local/tmp; chmod 777 mempodroid
```

* This is the important part. We execute the file, passing in the fixed exit
  locations, which allows us to remount the system partition as re-writeable.

```bash
$./mempodroid 0xd7f4 0xad4b mount -o remount,rw '' /system
```

* Now execute the shell command, which should run as the root user:

```bash
$ ./mempodroid 0xd7f4 0xad4b sh</pre>
```

* [You are now inside a root shell instance inside your phone] If the previous
  step was successful, your prompt should change to a ```#```. If it doesn't,
  you cannot continue. If it does, we can then write the update file to the
  cache partition:

```
# cd /sdcard; cat update.zip > /cache/update.zip
```

* Finally, type <code>exit</code> twice to leave the root shell instance and the
  ADB shell instance.

* [You are now back in your computer's shell] We are almost done. Now, restart
  the phone directly into recovery mode:

```bash
$ adb reboot recovery
```

* At this point, you should be looking at a **red exclamation point inside
  an Android icon**. This is normal. Hold down the Volume Up key and the
  Power button for a few seconds until a blue-text menu comes up.

* On that menu, you should see an option to flash a zip file. Click that using
  the Power button and then select the ```update.zip``` file that we just wrote
  to there.

* Once that is done, sit back and let the update take place. It might appear to
  freeze for several seconds at the very end. Don't touch it, just let it
  finish and restart the phone.

And that's it! It should be working fully. Enjoy your updated phone (and
safer kernel).

([Source][1])

 [1]:
 http://strifejester.wordpress.com/2012/05/01/imm76k-direct-for-verizon-galaxy-nexus/
