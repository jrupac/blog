+++
title = "Android Feedly App Leaking Pocket Password"
date = "2012-12-30"
tags = ["android", "feedly", "oauth", "password", "pocket", "privacy"]
+++

I had posted a few weeks ago on [Reddit][1] that the Feedly application on
Android was displaying the user's Pocket username and password in
plaintext on LogCat.

This is a problem on devices running any version of Android before 4.1, which
according to the [latest Dashboards refresh][2] is about 92% of devices. This is
because any user-level application on pre-Jellybean devices can access the main
LogCat stream and read it. In fact, I was able to verify that, without giving
root privileges, I could see my Pocket password within the aLogCat application
on an Android 2.3.3 device. After Jellybean, the functionality has been modified
so that an application can only see it's own LogCat stream, so this
problem is nullified there (it still exists, but it's not a security
issue).

I originally encountered the issue when I was examining LogCat to debug another,
unrelated issue with a different application. I was initially very impressed
with the response time of a Feedly representative when I cross-posted the issue
on [Get Satisfaction][3]. They mentioned that they had done a full code review
and realized that the information was posted due to a debug string that they had
left into the production build. I fully believe that this is possible since the
message itself just looks like a JSON-like format that contains a bunch of
information about a user. While it's true that Pocket now supports
[OAuth][4], it's understandable that the Feedly developers had not yet
migrated over to using it and therefore had possession of the password itself
(after all, we do directly input it into the application).

The problem however is that the promised update to resolve the issue - which was
said to happen the following week - never happened. It's now been 29 days since
the Feedly representative said that the app update should be out. While it's
true that it's the holiday season now, there were definitely at least three
weeks of time in between during which Feedly could have put out an update.

I'm not going to tell anyone what to do, but for me this is a sign that
they either do not take privacy as importantly as one might like or that they do
not think that this issue is that important. And for me, that's convinced
me to switch to a different application for my Google Reader news. It's a
shame because I really, really did like the Feedly interface.

 [1]: http://www.reddit.com/r/Android/comments/1427nm/fyi_feedly_app_displays_pocket_password_in/
 [2]: http://developer.android.com/about/dashboards/index.html
 [3]: https://getsatisfaction.com/feedly/topics/android_application_displays_pocket_password_in_plaintext_on_logcat?rfm=1
 [4]: http://getpocket.com/developer/docs/migrating-accounts
