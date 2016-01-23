+++
title = "Tasky: A CLI Google Tasks Client in Python"
date = "2011-06-01"
tags = ["google", "tasks", "Python", "programming"]
+++

I've been looking for a nice and fast Google Tasks client for a long time.
There do exist a lot of them but none that I have encountered matched what I was
looking for in terms of functionality and ease of use. Given that I have a bit
of free time this summer, I decided to write one myself.

This task would have been really hard and clunky a month ago. Luckily, Google
introduced the [Tasks
API](http://googleappsdeveloper.blogspot.com/2011/05/getting-organized-with-tasks-api.html)
about three weeks ago, a hugely requested feature. I hadn't worked with Google
APIs before so I took it up as a challenge to figure out how it all works and
put together my client in a few days' time. I also decided to go with Python
this time, as I'd like to get more practice with the language (which I this is
really amazing, by the way). The result of the past three days is a very simple
and fast client for Google Tasks that works in the command line.

## Goals

First, I had to define what I wanted to accomplish with this project. I wanted a
way to retrieve tasks from one or more task lists (I currently have one but have
used multiple in the past), have a way to quickly add, remove, and check/uncheck
tasks. I wanted the whole thing to work in the terminal, as that's where I
spend most of my time working. And I wanted the commands to be as short as
possible, as to minimize the amount of typing necessary. Specifically, I did
_not_ want to have to type an entire task's name in order to perform some action
on it. And I wanted the whole thing to take as little time as possible between
subsequent operations - I didn't mind a possibly slow shutdown if I could have
fast removes and adds and toggles while the application was open. Finally, I
wanted to have two modes: Normal Mode and Quick Mode. In Normal Mode, the
application is opened and kept running, allowing me to make a series of changes
to it before closing. In Quick Mode, I perform a single operation fully
specified as arguments in the command line. The changes are made (with
ambiguities resolved when necessary) and the application closed immediately. And
I wanted to have it do authentication in order to access account information
from any computer I choose to run the application on.

## Implementation

I was able to accomplish all of these goals by loading all task lists into
memory upon startup and committing all changes to the server when closing the
application. The Tasks API is still in Labs, which means it could change from
the time of this writing, but as of right now, retrieving everything requires
$N+1$ API calls, where $N$ is the number of task lists in a given account. So
that was fairly quick. I used Google's sample code almost verbatim for OAuth 2.0
authentication (yay security!) and that's usually the slowest part of the
application. It does cache credentials, but still takes a bit of time to
establish a secure connection. Once all the data is retrieved, I decided to
store it in memory as a `dict` of `dict` of lists of `task` objects -
which are just `dict`s themselves. That's a lot of dictionaries, but it made
sense since I was able to get to fast accesses, insertions, and deletions. As
part of the way I implemented the code, the dictionary of tasks had to be an
`OrderedDict`; if you look at the code, it'll make sense why. I then performed
changes on the in-memory task lists as the user made changes, but those changes
were only reflected via flag statuses and not actually performed. Then, when the
application is closed, I sent back just the changes, minimizing the number of
total API calls.

## Conclusion

The result is a little application that I'm fairly proud of. It does what
I need it to do and I actually do use it. There's currently no
documentation beyond what's in the source code and the code is still a bit
rough around the edges. The link to my repository is
[here](https://github.com/jrupac/tasky). As mentioned on that page, this
application requires Python >=2.7, Google API client for Python, and the
Fabulous library (used for having bold and strike-through text in the terminal).
I have not included the Client ID, Client Secret, or Developer Key values in
this code, but you can get those values by registering this application for
yourself via the Google API Console. The code itself is fully open-source and
you are free to use or modify it as you wish.  It's still in development, so
look out for changes in the future.
