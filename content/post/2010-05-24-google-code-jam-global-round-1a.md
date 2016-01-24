+++
title = "Google Code Jam Global Round 1A"
date = "2010-05-24"
tags = ["code jam", "google", "programming"]
+++

Okay so I didn't make this time. I had enough solutions right to qualify
but didn't do it fast enough. And considering that the competition was on
the same weekend that I had to move out of Princeton, I didn't have the
time or energy to do compete in Rounds 1B or 1C. But it's all okay because
I had a fun time and now summer is here! At least for the next two weeks before
I have to get back and start summer research.

But regardless, below I have posted my solution to the first problem of Round
1A. I had a somewhat fast algorithm but nothing too special. Regardless, here is
the source:

```java
import java.util.*;
import java.io.*;

public class Join
{
   private String filename;
   private char[][] board;

   public Join(String f)
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
            int N = s.nextInt();
            int K = s.nextInt();

            board = new char[N][N];

            for (int j = 0; j < N; j++)
            {
               String line = s.next();

               for (int k = 0; k < N; k++)
                  board[j][k] = line.charAt(k);
            }
            output(i + 1, process(K, N));
         }

         s.close();
      }
      catch (Exception e)
      {
         System.err.println("I/O error: " + e);
         System.exit(0);
      }
   }

   private String matchesRed(String tmp, String retval, int K)
   {
      String regexRed = ".*R{" + K + "}.*";

      if (tmp.matches(regexRed))
      {
         if (retval.equals("Red"))
            return retval;
         if (retval.equals("Neither"))
            retval = "Red";
         else
            retval = "Both";
      }

      return retval;
   }

   private String matchesBlue(String tmp, String retval, int K)
   {
      String regexBlue = ".*B{" + K + "}.*";

      if (tmp.matches(regexBlue))
      {
         if (retval.equals("Blue"))
            return retval;
         if (retval.equals("Neither"))
            retval = "Blue";
         else
            retval = "Both";
      }

      return retval;
   }

   private String process(int K, int N)
   {
      String retval = "Neither";
      gravity(N);

      // Check vertical
      for (int i = 0; i < N; i++)
      {
         String tmp = new String(board[i]);

         retval = matchesRed(tmp, retval, K);

         if (retval.equals("Both"))
            return retval;

         retval = matchesBlue(tmp, retval, K);

         if (retval.equals("Both"))
            return retval;
      }

      // Check horizontal
      for (int j = 0; j < N; j++)
      {
         String tmp = "";

         for (int i = 0; i < N; i++)
            tmp += board[i][j];

         retval = matchesRed(tmp, retval, K);

         if (retval.equals("Both"))
            return retval;

         retval = matchesBlue(tmp, retval, K);

         if (retval.equals("Both"))
            return retval;
      }

      // Check diagonal 1
      for (int i = 0; i < N; i++)
      {
         int orig = i;
         String tmp = "";

         for (int j = 0; i >= 0;)
            tmp += board[i--][j++];

         retval = matchesRed(tmp, retval, K);

         if (retval.equals("Both"))
            return retval;

         retval = matchesBlue(tmp, retval, K);

         if (retval.equals("Both"))
            return retval;

         i = orig;
      }

      int i = N - 1;

      for (int j = 0; j < N; j++)
      {
         int orig = j;
         String tmp = "";

         for (; j < N;)
            tmp += board[i--][j++];

         retval = matchesRed(tmp, retval, K);

         if (retval.equals("Both"))
            return retval;

         retval = matchesBlue(tmp, retval, K);

         if (retval.equals("Both"))
            return retval;

         i = N - 1;
         j = orig;
      }

      // Check diagonal 2
      for (int j = N-1; j >= 0; j--)
      {
         int orig = j;
         String tmp = "";

         for (i = 0; j < N;)
            tmp += board[i++][j++];

         retval = matchesRed(tmp, retval, K);

         if (retval.equals("Both"))
            return retval;

         retval = matchesBlue(tmp, retval, K);

         if (retval.equals("Both"))
            return retval;

         j = orig;
      }

      int j = 0;
      for (i = 0; i < N - K; i++)
      {
         int orig = i;
         String tmp = "";

         for (; i < N;)
            tmp += board[i++][j++];

         retval = matchesRed(tmp, retval, K);

         if (retval.equals("Both"))
            return retval;

         retval = matchesBlue(tmp, retval, K);

         if (retval.equals("Both"))
            return retval;

         j = 0;
         i = orig;
      }

      return retval;
   }

   private void gravity(int N)
   {
      int j;

      for (int i = 0; i < N; i++)
      {
         String tmp = "";

         for (j = 0; j < N; j++)
            if (board[i][j] != '.')
               tmp += board[i][j];

         int diff = N - tmp.length();

         for (j = 0; j < diff; j++)
            board[i][j] = '.';

         for (; j < N; j++)
            board[i][j] =  tmp.charAt(j - diff);

      }
   }

   private void output(int caseNum, String result)
   {
      System.out.println("Case #" + caseNum + ": " + result);
   }

   public static void main(String args[])
   {
      Join app = new Join(args[0]);
   }
}
```

So basically, I first took the input and put it inside a two-dimensional
character array. Then, I implemented gravity (if only it were so easy always!).
I made the observation that all gravity does is move the non-period characters
to the right side and pushes all intermediate period characters to the left.
Originally, I had a recursive algorithm to do this, but this was way simpler.
So, I parsed each line and collected the non-period characters in a temporary
string. Then, I added sufficient periods as padding and then just threw my
collected string at the end of each line. Do this `N` times, and you're done. Of
course, I could've used some regex fu to make this even faster, but it was
straightforward enough and did the job in $\mathcal{O}(N^2)$ which is the best
asymptotic performance you can get (gotta read every character..).

Then, I did a series of checks to see if any `K` consecutive letters had been
formed as a result. To do so, I decided that the simplest way to do so would be
to just read in my array vertically, horizontally, and by diagonals and check to
see a match. Rather than dealing with messy DFAs, a one-line regex check did the
trick. Here, the regex I used was constructed as the following: `".*R{" + K +
"}.*"` for the red pieces. It's a really simple one and basically it just looks
at an arbitrary number of lead characters (`".*"`) and then checks
for exactly `K` characters of value "R" and then ignores an arbitrary number of
characters that trail (`".*"`). Of course, the regex string has to actually
have a number in the place of `K`, which is why I constructed in this way.
Similarly, to check for blue pieces, I just used `".*B{" + K + "}.*"`.

You can check out the code to see how verbose, ugly, and unoptimized it is. But
given that the largest value for N was 50, doing about 5 traversals of a
50x50 grid was acceptable. And in fact I was correct on this point. For the
small input, the code run in 0.298s and for the large input, it runs in 0.980s.
Good enough.
