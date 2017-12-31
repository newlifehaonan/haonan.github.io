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

## Heap Sort
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
## Merge Sort
**_Mergesort use divide and conquer method to split the array into small pieces and sort each pieces then merge them together_**

### 1. Bottom-UP
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

}
```

### 2. Top-down
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
}
```

## Quick Sort

<hr>

_CopyRight &copy;Newlifehaonan.com_
