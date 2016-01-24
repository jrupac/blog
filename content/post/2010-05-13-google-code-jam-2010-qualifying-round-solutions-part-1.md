+++
title = "Google Code Jam 2010 Qualifying Round Solutions (Part 1)"
date = "2010-05-13"
tags = ["code jam", "google", "programming", "java"]
+++

Okay so now I'll post some of my solutions for Google Code Jam Qualifying
Round 2010. The first one was called the Snapper problem. You can read the
description of the problem [here][1].

This solution was a very, very simple brute force solution. It worked well for
the small input but obviously did not work for the large input. Anyway, here it
is:

<!--more-->

```java
import java.util.*;
import java.io.*;

public class Snapper
{
   String filename;

   public Snapper(String f)
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
            output(i + 1, process(s.nextInt(), s.nextInt()));

         s.close();
      }
      catch (Exception e)
      {
         System.err.println("I/O error: " + e);
         System.exit(0);
      }
   }

   private String process(int N, int K)
   {
      ArrayList<Snap> list = new ArrayList<Snap>();

      for (int i = 0; i < N; i++)
         list.add(new Snap());

      // first one has power
      list.get(0).flipPower();

      // snap, snap, snap yo fingers!
      for (int i = 0; i < K; i++)
      {
         // if it has power, flip its state
         for (int j = 0; j < N; j++)
         {
            if (list.get(j).getPower())
               list.get(j).flipState();
         }

         int j = 1;

         // turn on power of all those whose prev's
         // state and power is on
         for (; j < N; j++)
         {
            if (list.get(j - 1).getState() &amp;&amp;
                list.get(j - 1).getPower())
               list.get(j).onPower();
            else
               break;
         }

         // j is the index of the first snapper whose prev's
         // state is off
         // turn rest's power off
         for (; j < N; j++)
            list.get(j).offPower();
      }

      // check if all powers and all states are on.
      int i = 0;

      for (; i < N; i++)
      {
         if (!list.get(i).getState() || !list.get(i).getPower())
            break;
      }

      return (i == N) ? "ON" : "OFF";
   }

   private void output(int caseNum, String result)
   {
      System.out.println("Case #" + caseNum + ": " + result);
   }

   public static void main(String args[])
   {
      Snapper app = new Snapper(args[0]);
   }
}

class Snap
{
   private boolean state;
   private boolean power;

   public Snap()
   {
      state = false;
      power = false;
   }

   public void  flipState() { state = !state; }

   public void flipPower() { power = !power; }

   public boolean getState() { return state; }

   public boolean getPower() { return power; }

   public void offPower() { power = false; }

   public void onPower() { power = true; }

}
```

I know that it's super long and unnecessarily OOP'd, but it was my first
solution so I figured it was okay. The solution (for small input) ran in 0.647s
on an Intel Core 2 Duo 2.53 GHz, 4GB RAM, running Ubuntu 10.04.

The approach is pretty much self-explanatory so I won't go into the details of
it. It's also sorta commented so you can figure out what I did. Obviously, I
could have made it more efficient but it got me the answer quickly anyway so
life's good.

 [1]: http://code.google.com/codejam/
