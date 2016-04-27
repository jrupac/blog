+++
categories = []
date = "2016-04-27T03:20:06Z"
description = ""
keywords = []
title = "Prometheus and Grafana"

+++

### The SRE Book

I just got my copy of Google's new [Site Reliability
Engineering](http://g.co/SREBook) book.

I'm only a couple chapters and I'm already so happy that
we've made so much of what we do internally public. Reading through the thought
that went into the decisions made early on in the company's life that so
dramatically affects day-to-day operations, norms, and practices brings back the
same fascination I had when I first started there. I spent the first few months
reading through as much of the design docs and documentation of the major
underlying services and products as I could to try and begin to understand the
challenges that running so many massive services presents. It makes me so happy
that others may also benefit from seeing the outcomes and decisions for those
challenges.

## Nest-WFH

A couple months ago, I wanted to workaround the problem of my house being too
cold or too warm once I got home. I'd only be awake for a few hours in the
evening and turning on the heat/AC once I got home would mean that it'd only get
comfortable as I was heading to bed. I already had a Nest thermostat, but my
schedule was too erratic for it to reliably predict when I'd be home. I wanted
an energy-efficient and automated solution.

An idea I had was as follows. I use IFTTT on my phone to publish an event "I
entered work" or "I exited work" on entering/leaving a geofence around my
workplace on a dedicated calendar.


I then wrote a simple [script](https://github.com/jrupac/nest-wfh), intended to
run as a cron, that checks a designated Google Calendar and looks for events
that look like those above and set the Nest thermostat in my home to the
appropriate state so that my house is comfortable by the time I get home (it so
happens that my commute roughly matches how long it takes on average to bring my
house to the desired temperature). The script also ensures that the Nest doesn't
go back into "auto-away" mode again since it hasn't detected anyone physically
around for a while. The script runs every 5m on a GCE instance and worked
remarkably well for a while.

## Prometheus/Grafana

I then got it into my head that it'd be pretty neat to have some actual
monitoring and dashboard for the bits and pieces I have around home automation
(Amazon Echo controlling virtual switches created in SmartThings that in turn
proxy actions to a couple of lights and outlets, some more geofencing around my
house to turn on lights just as I pull up, etc.) I went looking for a good
open-source monitoring platform and stumbled upon Prometheus. When I first saw
it, I hadn't at all known that it was from ex-Googlers intending to imitate
Google's (now publicly known) Borgmon - but I did think the resemblance was
remarkable :).

I spent some time playing around with it, also on the same GCE instance as the
script earlier and within a couple hours I had it set up. I also had to set up a
pushgateway (essentially a separate process that receives exported (pushed)
metrics from ephermeral jobs like crons and then exposes them to be scraped by
Prometheus normally). I made some simple init.d scripts for these and set them
to run continuously in the background and come up on boot. Then with a bit more
effort, I was also able to set up Grafana to fetch and render metrics from
Prometheus. This was all super cool because with just a bit of effort I was able
to actually get some insight and metrics on how the temperature and humidity
changes over time, for example.

I still don't know how far I'm going to take all this. The main objective here
was to learn some new products and get an idea of what's available out there.
It's funny that I happened to come across something I work with a fair bit at
work, which ended up having the effect of making working with it totally simple.
The longer term vision is to have a unified logging service and metrics for all
important events regarding my home automation so that debugging when things
break (which.. they still do a fair bit) is much simpler. A lot of these things
are similar to what I'd do at work too to make a service stabler, which is why
maybe I'm attracted to this problem.

---

Later on, I realized that Nest introduced a new "ETA" feature that adds a signal
into Nest's algorithm for when to pre-heat/pre-cool a house based on estimates
on when the user will be home; the Nest phone app pushes this information
automatically so it does what my script does, except more precisely. I still
need to benchmark how well it works against my method, but this new monitoring
will help towards getting some numbers on this.

