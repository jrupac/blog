+++
title = "RIP Google Reader"
author = "Ajay Roopakalu"
date = "2013-03-15"
tags = ["amazon", "ec2", "google", "programming", "reader", "tt-rss"]

+++

Yesterday, we all heard the unexpected news that Google Reader would be shutting
down on July 1 of this year. While this probably should not have been too much
of a surprise given how little attention it was receiving, it was still a huge
shock to me nonetheless. I've used Google Reader nearly every day for the
last four years and at this point have read over 245,000 articles on there. But
all is not lost. I was able to do the Google Takeout to retrieve my
subscriptions (around 60 or so, so not too big) and I've been looking for
other options. While I mentioned Feedly negatively a while back, it seems like
they have made some major updates to their service and are looking to replace
the Reader backend with their own that supports the same API, so I have imported
and synced up my account with them for now. Further, apps like Press for Android
(which is my main RSS reader right now) are also looking into options to keep
their app going, so that will be interesting.

In the meantime, I've been wanting to move towards a more reliable
solution that does not depend on third parties and so one option that is working
remarkably well so far is [Tiny
Tiny RSS](http://tt-rss.org/redmine/projects/tt-rss/wiki). This is a self-hosted
solution that I've currently deployed on my EC2 instance. There's
also an associated Android application which is
reasonably good and so I'm using that as well. While this does increase
disk and net I/O, I think it might still worth it. I'm still working
through the kinks of doing it all so we'll see if this things work!

**Update**: After installing TT-RSS and using it for a few days, I think I
really like it! The developer is very active (especially with all the new users
coming in). I'm reasonably happy with how it looks on the web interface as
well as the Android app, although I'm thinking of doing a few more pull
requests with minor changes/fixes. I just hope that it will eventually support
the Reader API and in doing so, be able to connect to any RSS frontend. I
cautiously hopeful that this is the direction that RSS reading will go in the
future (multiple frontends, multiple backends, talking over a unified API). That
would be quite swell.
