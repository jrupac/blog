+++
title = "Potential in the Making"
date = "2011-04-21"
tags = ["algorithms", "potential", "theory"]
+++

$$
\Phi = \sum\_{e \notin C} n^{2w\_e}+n\exp\left(\frac{1}{2a}\left(\sum\_{s \notin C} c_s - \\sum_S 3x_s c_s \log(n)\right)\right)
$$

This was the equation my professor put on the board. Magically. If you've seen
algorithmic analysis before, you may recognize the form as a **potential
function**. Effectively, this is a very convenient and powerful way to perform
an amortized analysis on an otherwise difficult-to-analyze algorithm. Said
another way, it's a way by which we can keep track of money coming in and going
out of a metaphorical bank. Think of it this way: some weeks you need only \$5
dollars, every once in a while you need \$10 and one time you need \$500 for a
big expense. How much money do you usually need to go a week? A worst-case
analysis will yield \$500/wk, a gross over-estimate. A best-case analysis will
yield \$5, which doesn't come close to covering the \$500 you need that one
time. Instead, we look at it through a type of averaging that allows us to
retain the intricacies of the nature of the algorithm but more cleanly and
tightly express bounds on its performance.

Here's another example. Give a good estimate for a representative value of the
"ruler function". I put that in quotes because even I confused it with the more
famous function which has differing continuities at rational and irrational
numbers. The sequence I am here referring to is [Sloane
A001511](http://oeis.org/A001511/graph). A graph of the function below reveals
the reasoning behind the name.

{{% figure src="http://oeis.org/A001511/graph?png=1" %}}

Now once more, a worst-case analysis or best-case analysis fail terribly (giving
values of one and arbitrarily large as the sequence gets longer). But we can do
better and that therefore is the purpose of amortized analysis.

There's a great plenty that can be said about amortized analysis but I will
focus on one particular aspect here: how the hell did we get that thing on the
top of this page? I said it was magical but that was a lie. As it turns out, the
rationale for this particular function derives from its formulation via a
casting into a linear programming question. From this then were the
characteristic properties drawn and put together. The problem for which this
potential function applies is the Online Set Cover problem, which I will
describe quickly below.

## Online Set Cover

This is a pretty natural extension to a very common and simple problem called
the Set Cover problem. The basic idea is this: you'd like to cover a finite set
of elements using a subset from a set of subsets of the original elements.
Formally, given $E = \\{e\_1, \ldots, e\_n \\}$ and a family of subsets $S
=\\{S\_i\\}\_{i=1}^m$ with $S\_i \subseteq E$, with possible overlaps between
different sets. Each subset is assigned a non-negative weight $c\_i$ for each
$i=1, 2, \ldots, m$ and we wish to find the subset of family $S$ that both
covers every element in $E$ and has the minimum weight. In the online
variation of this problem, we are provided each $S\_i$ one at a time and we
must immediately update our current best minimum weight set cover. The potential
function above is used in the analysis of one algorithm to solve this problem.

The moral of the story, as I have learned through the course of this semester,
is that potential functions are not as free-form as I had first thought. I was
formally introduced to this type of analysis in both of my algorithms classes
this semester and through seeing the utilization of this technique time and time
again, I realize that reductions and re-castings of the problem in an entirely
unrelated framework can eek out insights that otherwise fare well in avoiding
the eye.

This is another part in my journey to learn about the structure of computation.
As corny as it may sound, I like to think that numbers, in accordance to the
laws of mathematics in this universe, hold deeper secrets than we like to
usually give them credit for. My real aim here is to unravel the complexities of
varied algorithms in different applications and find a common, basic,
irreducible element of them all. It wonders me to see how much more we can see
if we just look at a problem in the right way. While linear programming is
something I've come to gain a great respect for this semester, it's still only
the tip of the iceberg. There is an even broader scope by which we may express a
problem and perhaps other techniques will be the topic of future posts.
