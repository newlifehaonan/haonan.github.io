---
layout: post
title: "String Sort Algorithms"
date: 2018-3-5 15:00:00
tag:
- sort
description: String Sort, LSD,MSD, 3-way-quick-sort
comments: true
---

# String Sort

<hr />

# Appendix

* Key-indexed counting

* LDS

* MSD

* Three way quick-sort.

<hr />

# Key-indexed counting.

* counting the frequency of each key

**Initiate an array with first index blank, in our example, the key of item is a integer.**
```java
int N = a.length();
for(int i = 0; i < N; i++){
  count[a[i].key() + 1] ++;
}
```

* Replace count array with accumulate value

**The accumulate value is the index where the item at in aux array**

```java
int R = count[].length;
for(int r =0; r < R; r++){
  count[r+1] += count[r];
}
```

* distribute the item to the aux array

**Locate the data following the index position in count array**

```java
for(int i = 0; i < N; i++){
  aux[count[a[i].key()]++] =a[i];
}
```

<hr />

# LSD
**Do index counting from right to left in a string, this method only used for sorting strings that have same length.**
```java
public class LSD{
  public static void sort(String[] a, int W)
  {  // Sort a[] on leading W characters.
    int N = a.length;
    int R = 256;
    String[] aux = new String[N];
    for (int d = W-1; d >= 0; d--)
    { // Sort by key-indexed counting on dth char.
      int[] count = new int[R+1];     // Compute frequency counts.
      for (int i = 0; i < N; i++)
        count[a[i].charAt(d) + 1]++;
      for (int r = 0; r < R; r++)     // Transform counts to indices.
        count[r+1] += count[r];
      for (int i = 0; i < N; i++)     // Distribute.
        aux[count[a[i].charAt(d)]++] = a[i];
      for (int i = 0; i < N; i++)     // Copy back.
        a[i] = aux[i];
    }
  }
}
```

# MSD
**Designed for handling the unequaled length string and optimize effectiveness of sorting small string**
```java
public class MSD{
  private static int R = 256;// radix
  private static final int M = 15;// cutoff for small subarrays
  private static String[] aux;// auxiliary array for distribution

  private static int charAt(String s, int d)
  {  
    if (d < s.length()) return s.charAt(d); else return -1;  
  }

  public static void sort(String[] a)
  {
    int N = a.length;
    aux = new String[N];
    sort(a, 0, N-1, 0);
  }

  private static void sort(String[] a, int lo, int hi, int d)
  {  // Sort from a[lo] to a[hi], starting at the dth character.
    if (hi <= lo + M)
    {  
      Insertion.sort(a, lo, hi, d); return;  
    }
    /*Start Doing the index counting*/
    int[] count = new int[R+2];
    for (int i = lo; i <= hi; i++)
      // + 1 for the blanket cell in front, +1 for the -1 representation of end in tail
      count[charAt(a[i], d) + 2]++;
    for (int r = 0; r < R+1; r++)
      count[r+1] += count[r];
    for (int i = lo; i <= hi; i++)
      aux[count[charAt(a[i], d) + 1]++] = a[i];
    for (int i = lo; i <= hi; i++)     // Copy back.
      a[i] = aux[i - lo];

    for (int r = 0; r < R; r++)
      //the range is between the begin of new category and the end of this new category
      sort(a, lo + count[r], lo + count[r+1] - 1, d+1);
    }
}
```

# Three way quick sort

```java
public class Quick3String
{
  private static int charAt(String s, int d)
  { if(d <s.length() return s.charAt(d); else return -1;)}

  public static void sort(String[] a)
  {
    if(hi < lo) return;
    int lt = lo, gt = hi;
    int v = charAt(a[lo], d);
    int i = lo + 1;
    while(i <= gt)
    {
      int t = charAt(a[i],d);
      if(t < v) exch(a, i++, lt++);
      else if(t > v) exch(a, i, gt--);
      else t++;
    }

    sort(a, lo, lt-1,d); // sort the subarray which contains items that are less than piviot
    if(v > 0) sort(a, lt, gt, d+1); // sort the second letter
    sort(a, gt+1,hi,d)； //sort the subarray which contains items that are larger than piviot.
  }
}
```
