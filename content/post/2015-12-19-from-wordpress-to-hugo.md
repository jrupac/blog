+++
title = "From Wordpress to Hugo"
date = "2015-12-19T04:09:40Z"
tags = ["wordpress", "hugo", "go", "static"]

+++

I've recently got interested in static site generators as a consequence of
continously hearing about Wordpress hacks and continuous security updates. I've
gotten tired of figuring out how to deal with spam comments and having to think
about caching site contents (though this isn't a big site anyway so that isn't a
huge concern). I've also become tired of dealing with databases and PHP and
updating plugins and dealing with admin 2-factor authentication and everything
else.

Wordpress is a fantastic solution for a dynamic website but what I really wanted
was just a simple way to write posts and have everything be utterly quick and
(relatively) safe.

I investigated several different static generators but decided to not use
something with Ruby as I think that is already too heavy for what I need. There
were a few that used Node.js to generate content, which is neat, until I read
that it could take several minutes (!!) to build even a moderate site. No way. I
needed something that is very fast for both serving and building.

[Hugo](http://gohugo.io/) is a static site generator that uses the Go
programming language to build files. It can do incremental builds so that only
new files are generated, which lets everything happen within tens or hundreds of
milliseconds. That's a reasonable speed and in line with my expectations for
something like this.

It was fairly straightforward to install (with the exception that this Ubuntu
image I'm using had a too-old version of Go and this tiny GCE instance OOM'd
while trying to build Hugo from source.. oops). Downloading pre-built versions
of both solved that issue and after dealing with a few more small hiccups (fixed
after reading through the code), I got it all to work and am very impressed.
Even theming is built right into framework and applying your own changes on top
is fairly easy too (it'd be even cooler if you could cascade your own CSS files
on top of the themes, like you can in Wordpress, but Hugo only allows full file
replacement, which is functional too. I haven't played with the server that Hugo
comes with (which is supposed to be fast too) since I'm using an Apache setup
that also hosts my RSS feed aggregator (how I wish that could be like this..).

The last cool thing is that there's also importers from other blogging
frameworks to Hugo (several of the other static generators had similar things
too) so I was able to convert my old Wordpress blog's content to here. I still
have to fix some small things and will get that all online soon.
