+++
title = "Google Code Jam 2010 Qualifying Round Solutions (Part 3)"
date = "2010-05-13"
tags = ["code", "google", "jam", "java", "programming"]

+++

Finally, my solution for the Theme Park problem. For this one as well, I had a
somewhat optimized solution that brought down $10^8$ iterations to at most
2000, with an average case of around 500-750 iterations. The solution, again in
Java, is below:

<!--more-->

```java
import java.util.*;
import java.io.*;

public class Theme_large
{
   String filename;
   ArrayList<Group> groups;

   public Theme_large(String f)
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
         long R, k;
         int N;

         ct = s.nextInt();

         for (int i = 0; i < ct; i++)
         {
            R = s.nextLong();
            k = s.nextLong();
            N = s.nextInt();
            groups = new ArrayList<Group>();

            for (int j = 0; j < N; j++)
               groups.add(new Group(s.nextLong()));

            output(i + 1, process(R, k, N));
         }

         s.close();
      }
      catch (Exception e)
      {
         System.err.println("I/O error: " + e);
         System.exit(0);
      }
   }

   private long process(long R, long k, int N)
   {
      long r = 0;
      int n = 0;
      long totEarn = 0;
      long k_i;
      int n_start = 0;
      long first;

      if (N == 1)
         return groups.get(0).getSize() * R;
      // loop as many times as necessary to start at same spot
      // again.
      while (r < R)
      {
         k_i = 0L;
         n_start = n;

         // check if you've already started here - a cycle
         if (groups.get(n_start).isHit())
            break;

         // set earnings at start
         groups.get(n_start).setEarn(totEarn);

         while (true)
         {
            // if adding the next group will exceed k, break
            if (k_i + groups.get(n).getSize() > k)
            {
               totEarn += k_i;
               // hit the one you started on.
               groups.get(n_start).hit();
               groups.get(n_start).setR(r);
               break;
            }
            else
            {
               k_i += groups.get(n).getSize();
               n = (n + 1) % N;
            }

            // one group can't ride more than once at the same
            // time!
            if (n == n_start)
            {
               totEarn += k_i;
               // hit the one you started on.
               groups.get(n_start).hit();
               groups.get(n_start).setR(r);
               break;
            }
         }

         r++;
      }

      if (r == R)
         return totEarn;

      long loopCt = groups.get(n_start).getR();
      first = groups.get(n_start).getEarn();

      long quo = (R-loopCt) / (r-loopCt);
      // how many full cycles will exist; round down

      totEarn = first + ((totEarn - first) * quo);
      // find that totEarn
      r = loopCt + ((r-loopCt) * quo);
      // effectively performed that many cycles

      // do loop again to take care of remainder, ignore hits

      while (r < R)
      {
         k_i = 0L;
         n_start = n;

         while (true)
         {
            // if adding the next group will exceed k, break
            if (k_i + groups.get(n).getSize() > k)
            {
               totEarn += k_i;
               break;
            }
            else
            {
               k_i += groups.get(n).getSize();
               n = (n + 1) % N;
            }

            // one group can't ride more than once at the same
            // time!
            if (n == n_start)
            {
               totEarn += k_i;
               break;
            }
         }

         r++;
      }

      return totEarn;
   }

   private void output(int caseNum, long result)
   {
      System.out.println("Case " + caseNum + ": " + result);
   }

   public static void main(String args[])
   {
      Theme_large app = new Theme_large(args[0]);
   }
}

class Group
{
   private long size;
   private boolean hit;
   private long totEarn;
   private long r;

   public Group(long i)
   {
      size = i;
      hit = false;
      totEarn = 0;
      r = 0;
   }

   public void hit() { hit = true; }

   public boolean isHit() { return hit; }

   public long getSize() { return size; }

   public void setEarn(long e) { totEarn = e; }

   public long getEarn() { return totEarn; }

   public void setR(long rr) { r = rr; }

   public long getR() { return r; }

}
```

In this solution, I realized that we should just think of the line as a cyclic
graph component (or, equivalently, a circular linked-list) and think of the
roller coaster car as moving around the cycle. Eventually, I thought,
we're going to start someplace we started before. And if we do it once,
we'll probably do it again and again. But we already know how much money
will be earned by that! So, the code first begins executing normally, but
dropping "markers" every time it starts somewhere. If it sees that
it has already start here, we have just found ourselves a cycle! The additional
fields in the `Group` class just store information about how many rides there
are in a cycle and how much money is made in a cycle. From this information, we
can jump ahead many, many simulations of the roller coaster until we have fewer
rides remaining than the length of  a cycle. From this point on, the program
executes in a very simple fashion to the end. A slightly better approach would
be to store the amount made in each `Group` element and so compute the remaining
amount in constant time. But nevertheless, I was happy with this solution. The
solution, on a 2.53 GHz Core 2 Duo processor, ran in 0.442s.

And that concludes this Google Code Jam solution series.
