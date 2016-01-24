+++
title = "Google Code Jam 2010 Qualifying Round Solutions (Part 2)"
date = "2010-05-13"
tags = ["code jam", "google", "programming", "java"]
+++

Here's my solution to Fair Warning Large input. It's actually very
similar to my Small input solution except that `long`s are replaced with
`BigInteger`s (and it's designed to handle cases larger than 3. Anyway,
here's my solution:

<!--more-->

```java
import java.util.*;
import java.io.*;
import java.math.*;

public class Fair_large
{
   String filename;
   ArrayList<BigInteger> list;
   ArrayList<BigInteger> diffs;

   public Fair_large(String f)
   {
      filename = f;
      openFiles();
   }

   private void openFiles()
   {
      Scanner s;

      try
      {
         s = new Scanner(new File(filename));
         int ct;

         ct = s.nextInt();

         for (int i = 0; i < ct; i++)
         {
            list = new ArrayList<BigInteger>();
            diffs = new ArrayList<BigInteger>();
            int ct2 = s.nextInt();

            for (int j = 0; j < ct2; j++)
               list.add(new BigInteger(s.next()));

            output(i + 1, process(ct2));
         }

         s.close();
      }
      catch (Exception e)
      {
         System.err.println("I/O error: " + e);
         System.exit(0);
      }
   }

   private String process(int length)
   {
      Collections.sort(list);
      BigInteger T = BigInteger.ZERO;

      if (length == 2)
      {
         T = list.get(1).subtract(list.get(0));

         if (T.equals(BigInteger.ONE))
            return "0";

         return distToNextMultiple(list.get(0), T).toString();
      }

      for (int i = 1; i < list.size(); i++)
         diffs.add(list.get(i).subtract(list.get(i - 1)));

      while (diffs.size() > 1)
         diffs.add(0, diffs.remove(0).gcd(diffs.remove(0)));

      T = diffs.get(0);

      if (T.equals(BigInteger.ONE))
         return "0";

      return distToNextMultiple(list.get(0), T).toString();
   }

   private BigInteger distToNextMultiple(BigInteger x,
                                         BigInteger T)
   {
      return T.subtract(x.mod(T)).mod(T);
   }

   private void output(int caseNum, String result)
   {
      System.out.println("Case #" + caseNum + ": " + result);
   }

   public static void main(String args[])
   {
      Fair_large app = new Fair_large(args[0]);
   }
}
```

So in this solution, I did use some simple optimizations to reduce the amount of
actual computation needed to produce my solution. First, I handled the 2 case by
itself since you don't actually need to use GCD for that. I created an auxiliary
array called `diffs` that holds the differences between consecutive numbers (in
the sorted array). Thus, we know that adding some $y \geq 0$ does not change the
differences between them; so, if they must all be multiples of `T`, then their
differences must already be multiples of `T`. Then, the GCD of the all the
numbers of the auxiliary array is calculated. In my small input solution I wrote
a slightly optimized (non-recursive - to save on mem read/writes on the
stack) GCD function based on Euclid's algorithm. At some point I also used
Stein's algorithm for the GCD calculation. But for the large input, I decided to
just use the built-in GCD function that comes with `BigInteger`.

One key property that I exploited was that:

$$
\gcd(x\_1, x\_2, \ldots, x\_n) = \gcd(\cdots \gcd(\gcd(x\_1, x\_2), x\_3)\cdots)
$$

Thus, I computed the GCD of two consecutive numbers (the first two of the aux
array), removed both, and added in their GCD. Continuing in this way until there
was only one element left, I had computed the GCD of all the numbers with
reasonable speed. The other interesting function was the `distToNextMultiple`
function.

Mathematically, this function returns:

$$
(T-(x \mod T)) \mod T
$$

We know that if $x < T$ then $x \equiv x \mod T$ but otherwise, it is the
remainder after division by `T`. Subtracting this value from `T` gives how much
is left until the next multiple. But why the extra modulus `T`? If
$x \equiv 0 \mod T$, then $T - (x \mod T)$ yields $T$ which is wrong. Thus, the
extra modulus `T` takes care of this case.

And that's pretty much it. A pretty simple solution that works quickly. On the
same Core 2 Duo machine, the solution for the large input runs in 0.436s. Good
enough for me.
