---
layout: post
title: "Resized-array"
date: 2017-12-31 11:30:20
tag:
- Java
- data-structure
description: Resized array
comments: true
---
# Resized Array

<hr />

**_Resized Array is an extension of regular array, it auto load extra spaces for new items when index out of range or auto releases free memory space when actual number of item in array is small enough._**

* Constructor
  * `Arrays()`: create a Resized Array object.
* Methods
  * `isEmpty()` : return a boolean whether the array is empty.
  * `size()` : return the size of array.
  * `add(item A)` : add a new item A to array.
  * `find(item A)`: return the index of item A.
  * `remove(item A)`: remove item A from array.
  * `resize(int N)`: private method for auto resizing.

```java
public class Arrays<item extends Comparable<? super item>>{
  private int initsize;
  private item[] list;
  private int N =0;

  public Arrays(){
    initsize = 10;
    list = new item[initsize];
  }

  public int size(){
    return N;
  }

  public boolean isempty(){
    return N ==0;
  }

  public void add(item A){
    if(N >= list.length){resize(2*list.length);}
    list[N++] = A;
  }

  private void resize(int M){
    item[] newlist = new item[M];
    for(int i=0;i < N;i++){
      newlist[i] = list[i];
    }
    list = newlist;
  }

  public int find(item A){
    a = binarySearch(list,A);
    return a;
  }

  public void remove(item A){
    if(N>0 && N>list.length/4){resize(list.length/2);}
    int index = find(A);
    for(int i = index;i<N;i++){
      list[i] = list[i+1];
    }
    N--;
    list[N] = null;
  }

}
```
_CopyRight &copy;Newlifehaonan.com_
