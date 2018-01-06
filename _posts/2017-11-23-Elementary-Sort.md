---
layout: post
title: "4 Elementary Sort Algorithms"
date: 2017-11-22 16:25:06
tag:
- sort
description: Selection-Sort, Bubble-Sort,Insertion-Sort(Optimization), Shell-Sort
comments: true
---
# Elementary-Sort-Algorithm
<hr />

## Appendix
**(All sort algorithm are build in java environment)**

* [Elementary Sort](#Elementary-sort)
  * [Bubble-Sort](#Bubble-sort)
  * [Selection-Sort](#Selection-sort)
  * [Insertion-Sort](#Insertion-sort)
  * [Shell-Sort](#Shell-sort)

<hr />

## Elementary-Sort <a id ='Elementary-sort'></a>
**1. Selection-Sort<a id ='Bubble-sort'></a>**

_Start with the first item in Array, compare each of them to the rest of item in the array_

① Swap in-place : after each compare, swap the item right away
```java
public abstract class selection{

	  private static <T> void swap(T[] input,int i, int j) {
			T swap = input[i];
			input[i] = input[j];
			input[j] = swap;
	  }

	  private static <T extends Comparable<? super T>> boolean less(T[] input,int i, int j) {
		  if(input[i].compareTo(input[j])<0) return true;
		  else
			  return false;
	  }

    public static <T> void show(T[] a) {
    for (T element : a) {
      StdOut.println(element);
    }
    }

	  public static <T extends Comparable<? super T>> void sort(T[] input) {
		  for(int i = 0; i < input.length; i++) {
			  for(int j = i + 1; j < input.length; j++) {
				  int min = i;
				  if(less(input,j,i)) min = j;
				  swap(input,i,min);
			  }
		  }
	  }


	  public static void main(String[] args) {
		  Integer[] input = new Integer[10];

		  for(int i =0; i < 10; i++) {
			  input[i]=(StdRandom.uniform(9));
		  }

		  StdOut.println("Before Sort" );
		  show(input);

		  long start = System.currentTimeMillis();
		  sort(input);
		  long end = System.currentTimeMillis();
		  long cost = (end -start);

		  StdOut.println("After Sort");
		  show(input);
		  StdOut.println("Total time :");
		  StdOut.println(cost);
	  }

}

```
② Swap using index : swap items with "minimum index".
```java
public static <T extends Comparable<? super T>> void sort(T[] input) {
  for(int i = 0; i < input.length; i++) {
    for(int j = i + 1; j < input.length; j++) {
      int min = i;
      if(less(input,j,i)) min = j;
      swap(input,i,min);
    }
  }
}
```
**2. Bubble-Sort<a id ='Selection-sort'></a>**

_Start with the first item in array, compare each of them to the item next to it, put the larger one in behind_

```java
public class bubble {
    /*supportive method Block*/
	  private static <T> void swap(T[] input,int i, int j) {
			T swap = input[i];
			input[i] = input[j];
			input[j] = swap;
	  }

	  private static <T extends Comparable<? super T>> boolean less(T[] input,int i, int j) {
		  if(input[i].compareTo(input[j])<0) return true;
		  else
			  return false;
	  }

    public static <T> void show(T[] a) {
      for (T element : a) {
        StdOut.println(element);
      }
    }

  /*Sort method Block*/
	public static<T extends Comparable<? super T>> void  bubblesort(T[] input) {
		for ( int i = 0; i < input.length;i++) {
			for (int j = 0; j< input.length -i-1; j++) {
				if (less(input, j+1, j)) swap(input, j, j+1);
			}
		}
	}


    /*client block*/
	  public static void main(String[] args) {
		  Integer[] input = new Integer[10];

		  for(int i =0; i < 10; i++) {
			  input[i]=(StdRandom.uniform(9));
		  }

		  StdOut.println("Before Sort" );
		  show(input);

		  long start = System.currentTimeMillis();
		  bubblesort(input);
		  long end = System.currentTimeMillis();
		  long cost = (end -start);

		  StdOut.println("After Sort");
		  show(input);
		  StdOut.println("Total time :");
		  StdOut.println(cost);
	  }

}
```
**3. Insertion-Sort<a id ='Insertion-sort'></a>**

① Insert into the preorder sequence by comparing one by one

_assume the array behind the item is sorted, insert items that need to be sorted into the correct place in the sorted array_

```java
public class Insertion {
	  private static <T> void swap(T[] input,int i, int j) {
			T swap = input[i];
			input[i] = input[j];
			input[j] = swap;
	  }

	  private static <T extends Comparable<? super T>> boolean less(T[] input,int i, int j) {
		  if(input[i].compareTo(input[j])<0) return true;
		  else
			  return false;
	  }

    public static <T> void show(T[] a) {
  		for (T element : a) {
  			StdOut.println(element);
  		}
  	  }

	public static<T extends Comparable<? super T>> void  Insertionsort(T[] input) {
		for ( int i = 1; i < input.length;i++) {
			for (int j = i; j> 0 && less(input,j,j-1); j--) {
				swap(input,j,j-1);
			}

		}
	}

	  public static void main(String[] args) {
		  Integer[] input = new Integer[10];

		  for(int i =0; i < 10; i++) {
			  input[i]=(StdRandom.uniform(9));
		  }

		  StdOut.println("Before Sort" );
		  show(input);

		  long start = System.currentTimeMillis();
		  Insertionsort(input);
		  long end = System.currentTimeMillis();
		  long cost = (end -start);

		  StdOut.println("After Sort");
		  show(input);
		  StdOut.println("Total time :");
		  StdOut.println(cost);
	  }
}

```
② Insert into the preorder sequence by binary search
```java
public class Insertion {

    public static<T extends Comparable<? super T>> int binary(T[] input,int lo,int hi,T target) {

      int mid;

      if(hi == lo) {
        return lo;
      }

      mid = lo + ((hi - lo)/2);

      if(input[mid].compareTo(target) <0) {
        return binary(input, mid +1, hi,target);
      }
      else if (input[mid].compareTo(target) >0){
        return binary(input,lo,mid,target);
      }

      return mid;
    }

    public static<T extends Comparable<? super T>> void Insertiionbinary(T[] input){
      for(int i =1; i < input.length;i++) {
        T temp = input[i];
        int loc = binary(input,0, i,temp);
        for(int j = i -1;j >=loc;j--) {
          input[j + 1] = input[j];
        }
        input[loc] = temp;
      }
    }
    //we can also use java array class API //

    public static<T extends Comparable<? super T>> void Insertiionbinary2(T[] input) {
		  for(int i =1; i < input.length;i++) {
			  T temp = input[i];
			  int loc = Math.abs(Arrays.binarySearch(input, 0, i, temp) + 1);

			  System.arraycopy(input, loc, input, loc + 1, i- loc);

			  input[loc] = temp;
		  }
	  }

  public static <T> void show(T[] a) {
    for (T element : a) {
      StdOut.println(element);
    }
    }

    public static void main(String[] args) {
      Integer[] input = new Integer[10];

      for(int i =0; i < 10; i++) {
        input[i]=(StdRandom.uniform(9));
      }

      StdOut.println("Before Sort" );
      show(input);

      long start = System.currentTimeMillis();
      Insertiionbinary(input);
      long end = System.currentTimeMillis();
      long cost = (end -start);

      StdOut.println("After Sort");
      show(input);
      StdOut.println("Total time :");
      StdOut.println(cost);
    }
}
```
**4. Shell-Sort<a id ='Shell-sort'></a>**

_split the array into different size, apply Insertion-sort in each array until the array size become 1. (the sequence of step length should be 1,4,7,9,11,3*k+1_

```java
private static <T extends Comparable<? super T>> void shell(T[] input) {
  int step = 1;
  while(step < input.length) {
    step = 3 * step +1;
  }

  while(step >= 1) {
    for (int i = step; i < input.length; i++) {
      for(int j =i; j >= step && less(input,j,j-step); j-=step) {
        swap(input,j,j-step);
      }
    }
    step = step/3;
  }
}
```
<hr />

_CopyRight &copy; Newlifehaonan.com_
