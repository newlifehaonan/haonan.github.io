---
layout: post
title: "3 Faster Sort Algorithms"
date: 2017-11-29 15:17:00
tag:
- Java
- algorithm
- sort
- data-structure
description: Heap-Sort,Merge-Sort,Quick-Sort
comments: true
---
# Faster-Sort-Algorithms

<hr />

## Appendix
**(All sort algorithm are build in java environment)**
* [Heap-Sort](#Heap)
* [Merge-Sort](#Merge)
  * [Bottom-Up](#Bottom)
  * [Top-Down](#Top)
* [Quick-Sort](#Quick)

<hr />

## <a name="Heap">Heap Sort<a />
**_Use MinPQ to reconstruct input array and call `delmin` function to return the sorted array._**

```java
public class Heapsort<Key extends comparable<? super Key>>{
  private static MinPQ M_PQ;
  private static Key[] sorted;

  public static Key[] H-sort(Key[] input){
    int N = input.length();
    sorted = new Key[N];
    M_PQ = new MinPQ(N);

    for(i = 0; i<N; i++){
      M_PQ.insert(input[i]);
    }

    for(i=0; i < N; i++){
      sorted[i] = M_PQ.delMin();
    }

    return sorted;

  }

}
```
## <a name="Merge">Merge Sort<a />
**_Mergesort use divide and conquer method to split the array into small pieces and sort each pieces then merge them together_**

### <a name="Bottom">1. Bottom-UP<a />
**_Merge array from small sized to large size until size become `array.length/2`(iteration function)_**

```java
public class mergesort<Key extends comparable<? super Key>>{

  private static Key[] aux;

  public static void conquer(Key[] a, int lo, int mid, int hi){

    int i = lo, j = mid + 1;

    for(int i =0; i < a.length; i++){
      aux[i] = a[i];
    }

    //compare left items to right items
    for(int k = lo; k<a.length; k++){
      if(i > mid){a[k] = aux[j++];}
      if(j > hi) {a[k] = aux[i++];}
      if(less(aux[i],aux[j])){a[k] = aux[i++];}
      else                   {a[k] = aux[j++];}
    }
  }

  public static void bottomupdivide(Key[] a){
    int N = a.length;
    aux = new Key[N];
    for(int size = 1;size < N;size * = 2){
      for(int a = 0;a < N-size; a + = 2* size;){
        conquer(a, a+size-1,a+2*size-1);
      }
    }
  }

  private static boolean less(int i, int j){
    return i.compareTo(j)<0;
  }

}
```

### <a name="Top">2. Top-down<a />
**_Recursively sorting the array by sort left array first and then right array until recursive function hit the base._**

```java
public class mergesort<Key extends comparable<? super Key>>{

  private static Key[] aux;

  public static void conquer(Key[] a, int lo, int mid, int hi){

    int i = lo, j = mid + 1;

    for(int i =0; i < a.length; i++){
      aux[i] = a[i];
    }

    //compare left items to right items
    for(int k = lo; k<a.length; k++){
      if(i > mid){a[k] = aux[j++];}
      if(j > hi) {a[k] = aux[i++];}
      if(less(aux[i],aux[j])){a[k] = aux[i++];}
      else                   {a[k] = aux[j++];}
    }
  }

  public static void topdowndivide(Key[] a){
    aux = new Key[a.length];
    topdowndivide(a, 0, a.length-1)
  }

  public static void topdowndivide(Key[] a, int lo, int hi){
    if(lo >= hi) return;
    int mid = lo + (lo+hi)/2;
    topdowndivide(a,lo,mid);
    topdowndivide(a,mid+1,hi);
    conquer(a,0,mid,hi);
  }

  private static boolean less(int i, int j){
    return i.compareTo(j)<0;
  }
}
```

## <a name ="Quick">Quick Sort<a />
**_Quick Sort is the complement of merge sort, it sorts the array by choosing a partition position before which are all items that are less than the pivot, and after which are all items that are larger than the pivot. In other word, we find each items's right position in each recursive function call !_**
```java
public class quicksort<Key extends comparable<? super Key>>{

  public static void conquer(Key[] a){
    StdRandom.shuffle(a);
    sort(a,0,a.length -1);
  }

  public static void conquer(Key[] a, int lo, int hi){
    if(lo >= hi) return;
    int j = partition(a, lo, hi);
    conquer(a, lo, j-1);
    conquer(a, j+1, hi);  
  }

  public static int partition(Key[] a, int lo, int hi){
    int i =lo, j = hi +1;
    while(True){
      while(less(a[++i], a[lo])){
        if(i == hi) break;
      }

      while(less(a[lo], a[--j])){
        if( j == lo) break;
      }
      if(i >= j) break;
      swap(a,i,j);
    }
    swap(a,lo,j)
    return j;
  }

  private static void swap(Key[] a, int i, int j){
    Key[] temp = a[i];
    a[i] = a[j];
    a[j] = temp;
  }

  private static boolean less(int i, int j){
    return i.compareTo(j)<0;
  }
}
```
<hr>

_CopyRight &copy;Newlifehaonan.com_
