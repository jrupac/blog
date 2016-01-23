+++
title = "Google App Engine + Google Voice + Google Talk"
date = "2011-12-28"
tags = ["API", "app engine", "gmail", "google", "google talk", "google voice",
"programming", "Python", "xmpp"]
+++
## The Premise

The Google Voice application on iOS is fairly good, but imperfect. For example,
it doesn't allow the user to not display a message preview in
notifications (such as the lock screen) and kills unsent text in the input box.
I also wanted to consolidate my communications into one portable protocol that I
could use on a variety of clients. The most popular solution to this problem is
to re-route Google Voice texts through XMPP to Google Talk (or technically any
other XMPP communication either). To this end, there is a third-party service
called [GVMax](https://www.gvmax.com/) which serves this need fairly well.
However, I've gotten more conscious about security recently and wanted to have
finer control over this potentially sensitive data.  In addition, I often get
messages that GVMax is offline for periods of time as well as greatly delayed
and missed messages, which is something I wanted to reduce as well.

## Introducing GVCommander

This is a very quick and simple application that I wrote for the purposes listed
above. As it turns out, GVMax at least in part uses Google App Engine as well,
so my program is not too different from theirs (although of course theirs is a
whole more sophisticated in supporting multiple users, etc.). Also, another
reason for doing this is because I really wanted to get a taste of how App
Engine works, after hearing and reading so much about it over the years. Turns
out, it's pretty freaking amazing. For absolutely free, you can host up to
ten applications with extremely generous (free) usage limits per day. You can
also enable billing to get even higher amounts of usage. It's also
decently flexible (not entirely, but generally good enough) to write a variety
of applications. I had some code that was initally designed to run on my EC2
instance (more on that in a different post some time..) but just by chance I
happened to discover that App Engine supports XMPP. Viola! I did have an XMPP
Python library I was using, but this one made it even easier to use, as well as
some additional flexibility, such as setting custom JIDs (simplified, this is
just usernames of the "from" address), in order to follow the model of GVMax.

Essentially, what happens is that suppose 555 is a full-length phone number.
Then to send a message to my friend at phone number 555, I send a message to
+555@gv-commander.appspotchat.com. This is identical to the procedure used by
GVMax. Further, whenever I get a text from a number (say, 444), it will appear
as a chat from a user with address (JID) +444@gv-commander.appspotchat.com.

## Under the Hood

The model behind App Engine is that every user (or internal) request is
essentially either a POST or GET to a specific application-specific URL. Not all
of these URLs necessarily have to be user-facing either, but generally are.
Then, you install a handler (a class with the appropriate caller functions) for
each of those web hooks. A YAML configuration file sets this up. This is of
course an async model. If you want something more regular, you have the option
of background tasks (called "backends"), but these are not free for continuous
usage. You also have a helpful cron service that can be used to effectively do a
request to a specific URL at regular intervals, although the granularity of this
is only up to once per minute. You also have Task Queues, which are fairly new.
These are basically like background tasks in that you set them to execute async
but you have a lot of control about when and how often they happen. Google uses
a bucket and token algorithm to decide when to execute tasks. A model  I used in
a previous iteration was a hybridization of cron jobs and task queues, in which
at each minute I executed a cron job that would instantiate a recursive task
queue (called "task chaining") of some fixed length, which was to complete
before the minute was up. Task chaining just basically means that you add
yourself to the task queue at the end of the task handler (so that it get called
again). With a recursion depth set as a parameter that is decreased at each
call, I was able to effectively simulate a finite task chain, which was neat and
efficient (remember, DON'T use sleep in App Engine. It's silly to block CPU like
that and waste resources for everybody - rather, use task queues as you with
would with SIGALRM in Linux). However, this required repeatedly polling the
Google Voice inbox, which can get slow (upwards to almost 5 seconds per request)
since parsing the very deep HTML returned (heh, no API means screen scraping)
was slow, as well as JSON parsing on every single poll. I did use BeautifulSoup
for this, but it was just so inefficient that I switched to a different model.

Behind the scenes this is what is happening. Google Voice can be configured to
forward text messages to your GMail address (I really wish they'd at least
allow to forward to any specified email address or even more ideally, actually
have an API..). Then a filter in GMail forwards the messages to
mail@gv-commander.appspotmail.com. On the App Engine application, when an email
is received, it attaches the appropriate sender address and sends it off to the
appropriate address. Conversely, when a message is sent via Google Talk to the
App Engine application, it receives it and parses it and then attempts to do an
HTTP POST request to an authenticated Google Voice page to send the text.

This is the part which is a bit hacky.

The code for this is a modified and stripped-down version of the popular library
PyGoogleVoice, which provides a neat interface to communicate with Google Voice
via essentially screen-scraping. In my code, I also added some helper functions
to parse messages in the inbox, although I did not use this in the current
iteration. What happens is that you start an HTTPSConnection with "google.com"
and then do a POST request to the Google Voice login page. Send over your
credentials and the page responds with a session key in the page. Just storing
that session key and hot-plugging it as needed allows you to do authenticated
GETs and POSTs. Fairly straightforward and the code is pretty short too. The
beauty is that the App Engine version of Python has a custom ```fetch```
function, so that even though it's transparent to application, all the requests
are going through Google's scalable platform. I actually had not even realized
this while writing the application at first and came to know it later when I was
looking up URL fetching on App Engine.

The application also has some simple controls. For example, sending a message to
STOP@gv-commander.appspotchat.com will cease forwarding of messages until a
message is sent to START@gv-commander.appspotchat.com.

The code is available [here](https://github.com/jrupac/GVCommander).
