+++
title = "Google Code Jam Qualification Round 2012 Solutions"
date = "2012-04-15"
tags = ["2012", "algorithms", "code jam", "google", "java", "programming",
"Python"]
+++

So yesterday I had a few hours free and decided to try the annual Google Code
Jam qualification round. Typically, the questions at this stage are fairly
straightforward and most people get them. We only needed 20 points (out of 100
possible points) to qualify, which equates to solving the small instances of two
problems or both the small and large instance of one problem. I was able to
solve three problems (small and large) for a total of 60 points. But the last
one was quite challenging and I haven't been able to figure that one out
yet. Anyway, here are my solutions and a bit of analysis for the first three.

## Problem 1: Speaking in Tongues

This was actually a completely trivial question. I enjoyed it because they
really made the question a bit out of the box. The solution was actually almost
completely encoded in the problem statement and sample input! Once you made that
realization, writing the actual code to solve an instance of basically any size
was very easy. The main observation in this problem is that we are given the
three mappings in the problem statement as well as a bunch of mappings in the
sample inputs. We are told that the map is constant among all possible inputs
so, I decided the easiest way to proceed was to just hard-code these mappings
directly into my code. Doing a quick check revealed that we had received exactly
25 character mappings. At this point, I just looked through the list and
realized that the mapping $q \rightarrow z$ was the only one missing. Adding
that one in manually completed the cipher and you could just use that to
translate everything else. The code is below.

```python
# Global data structure to hold mappings
s = {}

# Add in each character of the first string
# mapped to each character of the second string
def build(a, b):
    global s
    for i,x in enumerate(a):
        s[x] = b[i]

# Simple translation
def solve(string):
    global s
    t = ''

    for x in string:
        # Prevent extra 'n' characters
        # from being printed
        if x not in s:
            continue
        else:
            t += s[x]
    return t

def output(N, ans):
    print 'Case #{0}: {1}'.format(N, ans)

def main():
    # Mappings given in problem statement
    build('a', 'y')

    build('o', 'e')

    build('z', 'q')

    # Mappings given in sample input
    build('ejp mysljylc kd kxveddknmc re jsicpdrysi',
          'our language is impossible to understand')

    build('rbcpc ypc rtcsra dkh wyfrepkym veddknkmkrkcd',
          'there are twenty six factorial possibilities')

    build('de kr kd eoya kw aej tysr re ujdr lkgc jv',
          'so it is okay if you want to just give up')

    # Missing mapping
    build('q', 'z')

    with open('A-small-attempt0.in', 'r') as f:
        N = int(f.readline())

        for i in xrange(N):
            output(i + 1, solve(f.readline()))

if __name__ == '__main__':
    main()
```

This code runs in 0.036s on a small instance on my computer (Core 2 Duo,
2.53GHz, 4GB RAM, running 64-bit Arch Linux). There was no large instance for
this problem.

## Problem 2: Dancing with Googlers

Another cool problem. This looks way harder than it actually is. At first I
thought about actually finding the distribution of scores among the three judges
and trying to combinatorially figure out the best way to allocate the
'surprising' sets. But then I thought about it some more and realized that
taking the remainder of the scores when divided by 3 (that is, the mod), gives
exactly three cases that can be handled independently.

In the first case when the scores are divisible by 3, we can only have the
following legal combinations: Suppose the total score is 18. Then, the
distribution must be either 6, 6, 6 or 6, 5, 7. Thus, the largest 'best' score
we can get is at most one above the floor of the mean. Also, when this happens,
it must be a surprising set.

For the second case, consider a total score of 19. The only valid scores are 6,
6, 7 or 5, 7, 7. In either case, we can produce a largest 'best' score of
exactly one above the floor of the mean again. We can make the set surprising or
not.

For the third case, consider a total score of 20. The only valid scores are 6,
6, 8 or 6, 7, 7. In the first case, we can achieve a largest 'best' score
of exactly two above the floor of the mean, but this requires a surprising set.
Alternatively, we can get a largest 'best' score of exactly one above
the floor of the mean, and this is not a surprising set.

Finally, observe that if the mean is above or equal to the 'best' score
value we are looking for, we can always have a score value of at least the
number want. And that's all the things we need to solve this problem. The code
is below.

```python
import sys

# Do the packing algorithm
def pack(x, p, surp):
    avg = x / 3
    rem = x % 3

    # Handle special case of a total
    # score of 0
    if x == 0 and p > 0:
        return False
    # If the mean is at least the
    # score we want, we can always do it
    if p <= avg:
        return True

    # Exactly divisible
    if rem == 0:
        # We can do it when its exactly one
        # larger, but requires a surprising set
        return (p - avg == 1) and surp
    if rem == 1:
        # We can do it when its exactly one
        # larger, with or without surprising set
        return (p - avg == 1)
    if rem == 2:
        # If allowed a surprising set, we can do
        # do either 1 or two larger
        if surp:
            return (p - avg >= 2)
        # Otherwise, we can definitely do one larger
        else:
            return (p - avg == 1)

def solve(N, S, p, t):
    count = 0

    for x in t:
        # First try to pack without allowing a
        # surprising set
        if pack(x, p, False):
            count += 1
        # If that fails, try a surprising set
        # if we still have them left
        elif S > 0 and pack(x, p, True):
            S -= 1
            count += 1

    return count

def output(N, ans):
    print 'Case #{0}: {1}'.format(N, ans)

def main(fname):
    with open(fname, 'r') as f:
        N = int(f.readline())

        for i in xrange(N):
            a = [int(x) for x in f.readline().split(' ')]
            output(i + 1, solve(a[0], a[1], a[2], a[3:]))

if __name__ == '__main__':
    main(sys.argv[1])
```

This code runs in 0.038s for a small instance and 0.050s on a large instance.

## Problem 3: Recycled Numbers

There's probably a faster algorithm than the one I did (which was essentially a
naive one). In fact, based on some quick testing, my original solution in Python
would work fine for the small instances, but was taking too long for the large
instances. So I just rewrote it in Java and it worked fine (lol). Anyway, all I
did here was iterate over all values of ```n```, ranging from ```A``` to
```B```. Then, I enumerated all possible values of `m` derivable from it. I did
a few checks to ensure that I was not creating a number with leading zeros and
that the number was actually larger too. Finally, I also kept a ```HashSet```
for each ```n``` because there could have been duplicates (as evidenced by the
last sample input). And that's all there is to it. The source code is below.

```java
import java.util.*;

public class Recycled {
    private static int solve(int A, int B) {
        int count = 0;
        HashSet<Integer> s = new HashSet<Integer>();

        for (int n = A; n < B; n++) {
            String i = n + "";
            // Clear each time instead of re-allocating
            // to reduce GC and other overhead
            s.clear();
            int t = i.length();

            // Don't even bother with concatenations of
            // leading zeros
            while (i.charAt(t-1) == '0')
                t--;

            // Iterate over all possible valid concatentations
            for (int j = 1; j < t; j++) {
                // Do the swap (this is slow, but the numbers are small)
                int m = Integer.parseInt(i.substring(j) + i.substring(0, j));
                // Is it within the range specified?
                if (n < m && m <= B)
                    // Add to a set to remove duplicates
                    s.add(m);
            }

            count += s.size();
        }

        return count;
    }

    public static void main(String args[]) {
        Scanner in = new Scanner(System.in);

        int N = in.nextInt();

        for (int i = 0; i < N; i++) {
            System.out.println("Case #" + (i+1) + ": " +
                               solve(in.nextInt(), in.nextInt()));
        }
    }
}
```

This code ran in 0.488s (!) on a small instance and 27.760s (!!!) on a large
instance. Not great performance at all, but it works.

## Problem 4: Hall of Mirrors

I still have to think about this one. I have a few approaches such as recursive
back-tracking, but there's gotta be a more elegant way of looking at it. Didn't
make an attempt for this one.
