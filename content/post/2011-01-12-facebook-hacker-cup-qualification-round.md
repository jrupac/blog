+++
title = "Facebook Hacker Cup Qualification Round"
date = "2011-01-12"
tags = ["facebook", "hacker cup", "java", "programming"]
+++

So. I have a final later today and I'm too bored of studying so I decided
to write up a post on the latest Facebook Hacker Cup competition problems. As
you may know, this past weekend, Facebook had the Qualification Round of their
Hacker Cup. Basically, it's a programming contest very similar to that of
Google Code Jam that I mentioned a while back and follows a pretty regular
format.

In this round, we only had to answer one question out of three correctly to
qualify. I did qualify so I know I got at least one right. Strangely, they only
tell you the results after the competition is over, which is both weird and
annoying. Oh and they messed up sending out the emails to the people who
qualified. I made an incredibly dumb error in that I accidentally downloaded the
input file before I was done writing my code and so the 6-minute time limit
expired before I could submit my result. Fortunately, Facebook lifted this
six-minute restriction due to bugs in their system so my (eventual correct)
solution was actually accepted. The problem description and my solution is
listed below:

## Double Squares

I can't actually access the original question right now so I'm
paraphrasing here. Effectively, we are given an input file with the first line
being **N**, the number of lines that follow. Each of those lines contain an
integer between 0 and 2147395600, inclusive. (Not 100% sure if that's the
right number exactly but its within that range). The solution should contain one
integer per line of input containing the number of ways that the number can be
written (order doesn't matter) as a sum of two squares.

## Solution

There was a very nice little hint in light gray at the top of the problem page
that effectively said:

> too hard for brute force, switch to dp

Nice. Gladly, I saw that and immediately switched to a smarter algorithm (I
wasn't sure how hard these problems were and I wasn't going to waste
time writing a sophisticated algorithm if it was unnecessary - although my
solution is pretty straightforward for this problem).

First, I used a calculator to compute the square root of that number. It was in
the order of about 46,000, which is still small enough that pre-computing a data
structure of that size is not out of the question. Further, since that number
fits comfortably within the range of double-precision numbers, there was nothing
special I needed to do. I started off by generating an `ArrayList<Double>` of
each of the numbers from 0 to 46,342 squared. This actually executed faster than
I thought it would and so I continued along this idea. (Note: For use in the DP
algorithm I will describe, a `HashSet` would be a faster data structure although
since I couldn't perform a binary search on it, I decided to use an
`ArrayList`. Since this is performed only once per input line, I didn't
stress too much about it.) Next, having performed the binary search of the input
number on my giant list, I computed and stored the index of the greatest value
less than or equal to the number itself.

With this index, I created a second array which contained numbers only less than
or equal to it. The logic here is that since all our numbers are non-negative
(indeed, the squares of all real numbers are non-negative), each of the addends
must be less than or equal to the number (remember, 0 is on the list too). Now
having generated this sub-array, I traverse it and subtract each element of it
from the input number itself. This is effectively subtracting one of the squares
from the number. What is the reason for this? To check if the remaining quantity
is a perfect square and if so, to note that the resulting pair is one that we
are seeking. I used some quick and dirty counting scheme of adding a half for
each time I saw this (since we will be hitting upon commutative pairs) and
adding a whole number for the number itself. Then we simply return the count
that we have accumulated.

Very simple algorithm and runs pretty fast. We do a constant amount of
pre-processing so total time for that. Now if our number is $K$, then binary
searching for it runs in time complexity $\mathcal O (1)$ time (remember, the
size of the big array is fixed). Creating the sub-array and traversing it and
subtracting it from the original number runs in $\mathcal O (\sqrt{K})$ time and
traversing the list to search for numbers in the original list runs in
$\mathcal O (\sqrt{K} * 1) = \mathcal O (\sqrt{K})$ time (remember again, the
big array has a fixed size!). Thus, our total running time complexity for each
query (input number) is $\mathcal O (\sqrt{K})$. One optimization would be to
not search the big list for a match for every number in the sub-array but rather
just until $\lfloor \frac{\sqrt{K}}{2} \rfloor$ indices and then multiplying the
count by two.  In addition, if I had used a `HashSet` for the big array instead,
we would enjoy amortized constant time searching which would give us the same
big-Oh complexity, but much faster if you write it in tilde notation.

```java
import java.util.*;
import java.io.*;

public class Main {
  int SIZE = 46342;
  ArrayList<Double> squares;

  public Main() {
    squares = new ArrayList<Double>();
    for (int i = 0; i < SIZE; i++)
      squares.add(Math.pow(i, 2));
    solve();
  }

  private void solve() {
    Scanner in = new Scanner(System.in);
    int N = in.nextInt();
    for (int i = 0; i < N; i++)
      output(in.nextDouble());
  }

  private void output(double i) {
    if (i <= 1) {
      System.out.println("1");
      return;
    }

    double num = 0.0;
    int index = Collections.binarySearch(squares, i);
    if (index < 0)
      num++;
    index = (index <= 0) ? index: Math.abs(index) - 1;

    ArrayList subList = new ArrayList();

    for (int j = 1; j < index; j++)
      subList.add(i - squares.get(j));

    for (int j = 0; j < index - 1; j++)
      if (Collections.binarySearch(squares, subList.get(j)) >= 0)
        num += 0.5;

    System.out.println((int)num);
  }

  public static void main(String args[]) { 
    Main app = new Main(); 
  }
}
```

On my computer, running Ubuntu 32-bit and OpenJDK with Core 2 Duo processors at
2.53GHz, the total time taken to run this program on the input file given was
0.264s.
