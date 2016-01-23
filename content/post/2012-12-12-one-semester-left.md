+++
title = "One Semester Left!"
author = "Ajay Roopakalu"
date = "2012-12-12"
tags = ["f#", "functional", "haskell", "ocaml", "programming"]

+++

This week is the last week of my last fall semester in my undergraduate career.
That's pretty exciting. I took a bunch of really neat classes -
including a very neat one on functional programming.

I've been interested in FP for quite a while since I quite like the
mathematical elegance of it applied to a now-quite-different field of software
engineering. The language for the course was OCaml, which is something I wanted
to learn anyway. The only other FP language that I've formally learned in
Standard ML, so it was not too different from that and that made learning OCaml
much, much easier.

In a lot of ways, my impression is that OCaml is a more grown-up version of SML.
But in other instances, I'm still left thinking of it as a still-baby
version of a language like Haskell or F#. I have a little bit of experience in
the former of those due to personal interests and delving and virtually no real
experience writing in the latter. However, I am familiar with the architectures
of them a little bit, which is the backing behind that statement. The clearest
example of why I think OCaml is still a baby language to some
degree is its lack of a true parallel runtime system, due to its lack of a
parallel GC. This is the same issue with Python and I equivalently share the
same opinion about Python (now at least.. my opinions on PLs change quite a bit
over time). This is something which has been discussed to death and there are
Real People (TM) out there using the language in production, so it's not
entirely fair to call it a baby language.

Nonetheless, it's disappointing to see that this language (within a single
runtime) is almost diametrically opposed to the processor trend of increasing
the number of the cores (and that trend seems like it will last for a few more
years until we starting brushing up against limitations via physics laws again).
 What's more troubling to me though is that it's not just a TODO
that no one ever got around to - it's actually a design goal of the
language. Even weirder to me, the GC currently implemented is the
single-threaded version of the originally parallelized version designed by one
of the language creators that no one could apparently figure out how to
implement efficiently (at least that's my understanding from what we
discussed in the course I took this semester). The type system is quite
powerful, but there too there are some places (like existential types, for
example) that other languages are more powerful. And Haskell just destroys here
with the completely insane (in a good way) power that its type system has. There
are some annoyances too. Like how field names within records must be unique
among all fields in a module. Gr! I get the reasoning behind it, but its
nonetheless a silly limitation I think. To my knowledge, the GHC compiler, as
well as the MSIL compiler for F# are better optimized than the standard OCaml
compiler too. This I can see as something that will improve over time, so I
don't think that's too bad.

I do love all the niceties you automatically get from using a FP language
though. Like nice looking lambda expressions (which are actually full-fledged
functions rather than Python's crippled version):

```ocaml
let foo = (fun x -> print_string "yay!"; x + 1) ;; (* Side-effects! *)
let bar = foo 3 ;; (* prints "yay!" and sets bar = 4 *)
```

However, I'll also probably always be enamored with Haskell's `x ->
x + 1` syntax for anonymous functions (but the idea that a backslash is a
stand-in for the Greek letter lambda will forever remain corny). The fact that
we have algebraic types makes life approximately infinitely easier when I had to
implement a 2-3 tree this semester, which is basically like a dozen different
cases when doing re-balancing. Doing a `match` over them was beautiful, as
compared to the mess that an imperative language would have required. Currying
and partial function application also make module implementations that much
cooler and easier to write too. But I also spent a lot of time missing
Haskell's ```<$>``` and ```<.>``` operators for function
composition without the use of annoying parentheses.

Next semester, I'll be taking a graduate course on advanced programming
languages theory (essentially type theory). This is just for fun and I'm
very much looking forward to it! I've been reading up on un/typed lambda
calculus and refreshing my knowledge of some basic logic which I understand will
be useful. That class is mostly language-agnostic, so I won't get a chance
to learn another FP or anything. However, if time permits, I do want to get
better at Haskell and maybe check out F# as well a bit. Lots to learn!
