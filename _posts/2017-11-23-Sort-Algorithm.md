---
layout: post
title: "Collection of Sort Algorithm"
date: 2017-11-22 16:25:06
tag: Java algorithm Sort
description: Sort algorithm
---
# Sort-Algorithm
<hr />

##Appendix
**(All sort algorithm are build in java environment)**

* [Elementary Sort](#Elementary-sort)
  * [Bubble-Sort](#Bubble-sort)
  * [Selection-Sort](#Selection-sort)
  * [Insertion-Sort](#Insertion-sort)
  * [Shell-Sort](#Shell-sort)
* [Heap sort](#Heap-sort)
* [Merge sort](#Merge-sort)
* [Quick sort](#Quick-sort)

<hr />

##Elementary-Sort<a id ='Elementary-sort'></a>
**1. Selection-Sort<a id ='Bubble-sort'></a>**

_Start with the first item in Array, compare each of them to the rest of item in the array_

```java
public class selection {

	  private static void swap(ArrayList<Integer> input,int i, int j) {
		  Integer temp;
		  temp = input.get(i);
		  input.set(i, input.get(j));
		  input.set(j, temp)	;
	  }

	  private static boolean less(ArrayList<Integer> input,int i, int j) {
		  if(input.get(i).compareTo(input.get(j))<0) return true;
		  else
			  return false;
	  }

	  public static void sort(ArrayList<Integer> input) {
		  for(int i = 0; i < input.size(); i++) {
			  for(int j = i; j < input.size(); j++) {
				  if(less(input,j,i)) {
					  swap(input,i,j);
				  }
			  }
		  }
	  }

	  public static void main(String[] args) {
		  ArrayList<Integer> input = new ArrayList<Integer>();

		  for(int i =0; i < 50; i++) {
			  input.add(StdRandom.uniform(100));
		  }

		  StdOut.println("Before Sort" + input);

		  long start = System.currentTimeMillis();
		  sort(input);
		  long end = System.currentTimeMillis();
		  long cost = (end -start);

		  StdOut.println("After Sort" + input);
		  StdOut.println(cost);
	  }
}

```

**2. Bubble-Sort<a id ='Selection-sort'></a>**

_Start with the first item in array, compare each of them to the item next to it, put the larger one in behind_

**3. Insertion-Sort<a id ='Insertion-sort'></a>**

_assume the array behind the item is sorted, insert items that need to be sorted into the correct place in the sorted array_

**4. Shell-Sort<a id ='Shell-sort'></a>**

_split the array into different size, apply Insertion-sort in each array until the array size become 1. (the sequence of step length should be 1,4,7,9,11,3*k+1_

<hr />

##Heap-Sort<a id ='Heap-sort'></a>

_Heap structure is a data structure that each node are linked with the other two nodes, and each parent node needs to be either larger or smaller than its child nodes._

**1. Max-Priority-Queue**

**2. Min-Priority-Queue**

**3. Heap-base-Sort**

<hr />

##Merge-Sort<a id ='Merge-sort'></a>
**1. Divide**

**2. Conquer**

<hr />

##Quick-Sort<a id ='Quick-sort'></a>
**1. Divide**

**2. Conquer**
