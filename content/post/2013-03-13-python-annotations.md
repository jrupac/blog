+++
title = "Python Annotations"
author = "Ajay Roopakalu"
date = "2013-03-13"
tags = ["annotations", "decorators", "github", "programming", "Python", "type"]

+++

One of the most annoying (lack-of, in my opinion) features of Python is
it's dynamic typing. While this is wonderful for quick and dirty things
that you don't want to write lots of code for, it fails pretty much
horrifically for large, production-scale code. As I had the opportunity to
experience during the summer, when dealing with a project of several thousand
interleaving lines of Python, no one can help you figure out what the arguments
to a function should actually be (except for docstrings and humans, mostly
yourself).

That's a lot of thinking and storage of data in my head which I think is
unnecessary, time-wasting, and potentially dangerous (even good code coverage
with tests can still allow a slim corner case to occur and the lack of strong
compile-time checks for silly things like type checking keeps me up at night).
So when I came across a blog post today on Python type annotations, that sounded
just wonderful (how did I miss this before?!). However, after reading more about
it, it seems like Python doesn't go very far to actually do anything with
these annotations, especially at compile-time, so you're
left to do the grunt work yourself. But that's fine! It's better
than nothing.

The link to the blog post and the author's preliminary implementation of
the actual checking (done reasonably elegantly during decorators, which is as
good as I guess we can hope for in Python) are below:

[A powerful unused feature of Python: function
annotation](http://ceronman.com/2013/03/12/a-powerful-unused-feature-of-python-function-annotations/)

[GitHub implementation of the @typechecked
decorator](https://github.com/ceronman/typeannotations)
