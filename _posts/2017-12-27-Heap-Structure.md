---
layout: post
title: "Heap Structure"
date: 2017-12-27 11:30:20
tag:
- Java
- algorithm
- data-structure
description: Heap (Priority Queue) and its Extension)
comments: true
---
# Heap Structure
<hr />

## Appendix
**(All sort algorithm are build in java environment)**

* [Binary-Heap Definition](#Defination)
* [Type of Heap](#Type)
  * [MaxPQ](#MaxPQ)
  * [MinPQ](#MinPQ)
* [Heap Structure Extension](#Extension)
  * [Multiway Heap](#Multiway)
  * [resized Heap](#Resized)
  * [Index Heap](#Index)

<hr />

## <a name = "Defination">Binary-Heap Defination<a/>

**_Binary Heap is a structure within which the order of items are following the pattern that each item as a node should have at most two child nodes and its parent node should always be either greater or smaller than itself. If greater, the heap is called Max Priority Queue, same, if smaller, the heap is called Min Priority Queue_**

<hr />

## <a name = "Type">Type of Heap<a/>

### <a name = "Type">1. MaxPQ<a/>

Application interface includes:
* Constructor

  * `MaxPQ(int M)`: create a PQ has capacity M.

* Public method

  * `isEmpty()`: return whether the PQ is empty.

  * `size()`: return the size of PQ.

  * `max()`: return the item with highest priority.

  * `insert(key v)`: insert v into PQ.

  * `delMax()`: remove and return the highest priority key in PQ.

* private method to construct API

  * `exchange(int i, int j)`: exchange PQ[i] and a[j]

  * `less(int i, int j)`: return whether PQ[i] is less then PQ[j].

  * `swim(int v)`:move up PQ[v] to the correct position.

  * `sink(int v)`:move down PQ[v] to the correct position.

``` java
public class MaxPQ<Key extends comparable<? super Key>>{
  private  Key[] PQ;
  private int N ;
  //Constructor, create a MaxPQ with M capacity//
  public MaxPQ(int M){
    PQ = new Key[M+ 1];
    PQ[0] = null;
  }

  //insert a new item to MaxPQ//
  public void insert(Key v){
    N = N + 1;
    PQ[N] = v;
    //swim up new item to it's right place//
    swim(N)
  }

  //delete a the max item from MaxPQ//
  public Key delmax(){
    Key Max = PQ[1];
    exchange(1,N);
    N =  N -1;
    sink(1);
    //release the memory//
    PQ[N+1] = null;
    return max;
  }

  public Key max(){
    return PQ[1];
  }

  public int size(){
    return N;
  }

  public boolean isEmpty(){
    return N == 0;
  }
  //the following are the supportive function//
  private void exchange(int i, int j){
    Key temp = PQ[i];
    PQ[i] = PQ[j];
    PQ[j] = temp;
  }

  private boolean less(int i, int j){
    return PQ[i].compareTo(PQ[j])<0);
  }

  //v/2 is the parent index of node whose index is at v//
  private void swim(int v){
    while(v>1 && less(v/2, v)){
      exchange(v/2,v);
      v = v/2;
    }
  }

  private void sink(int v){
    while(v < N){
      int j = 2*v;

      if(j < N && less(j,j+1)) j++;

      if(less(v,j)){
        exchange(v,j);
        v = j;
      }
    }
  }
}
```

### <a name = "MinPQ">2.MinPQ<a/>

* Constructor

  * `MinPQ(int M)`: create a PQ has capacity M.

* Public method

  * `isEmpty()`: return whether the PQ is empty.

  * `size()`: return the size of PQ.

  * `min()`: return the item with lowest priority.

  * `insert(key v)`: insert v into PQ.

  * `delMin()`: remove and return the lowest priority key in PQ.

* private method to construct API

  * `exchange(int i, int j)`: exchange PQ[i] and a[j]

  * `less(int i, int j)`: return whether PQ[i] is less then a[j].

  * `swim(int v)`:move up PQ[v] to the correct position.

  * `sink(int v)`:move down PQ[v] to the correct position.

```java
public class MinPQ<Key extends comparable<? super Key>>{
  private  Key[] PQ;
  private int N ;
  //Constructor, create a MinPQ with M capacity//
  public MinPQ(int M){
    PQ = new Key[M+ 1];
    PQ[0] = null;
  }

  //insert a new item to MaxPQ//
  public void insert(Key v){
    N = N + 1;
    PQ[N] = v;
    //swim up new item to it's right place//
    swim(N)
  }

  //delete a the min item from MaxPQ//
  public Key delmin(){
    Key Min = PQ[1];
    exchange(1,N);
    N =  N -1;
    sink(1);
    //release the memory//
    PQ[N+1] = null;
    return max;
  }

  public Key min(){
    return PQ[1];
  }

  public int size(){
    return N;
  }

  public boolean isEmpty(){
    return N == 0;
  }
  //the following are the supportive function//
  private void exchange(int i, int j){
    Key temp = PQ[i];
    PQ[i] = PQ[j];
    PQ[j] = temp;
  }

  private boolean less(int i, int j){
    return PQ[i].compareTo(PQ[j])<0);
  }

  //v/2 is the parent index of node whose index is at v//
  private void swim(int v){
    while(v>1 && less(v, v/2)){
      exchange(v/2,v);
      v = v/2;
    }
  }

  private void sink(int v){
    while(v < N){
      int j = 2*v;

      if(j < N && less(j+1,j)) j++;

      if(less(j,v)){
        exchange(v,j);
        v = j;
      }
    }
  }
}
```

## <a name = "Extension">Heap Structure Extension<a/>

### <a name = "Multiway">1. Multiway Heap<a/>
**_unlike binary heap, multiway heap increase each node's child from two to any number you want.The main modification of the code lies in `swim` and `sink` functions, specifically the algebra relation between parent and child nodes. Following is implementation of d-way-heap_**

**_There is a little trick in the implementation of d-way-heap, we can use the bottom-down truncation character in java when two integers divided result in float point, for example, 1/3 and 2/3 both equals 0_**

```java
public class MinPQ<Key extends comparable<? super Key>>{
  private final int d; 			//Dimension of the heap
  private int N =0;						//Number of keys currently in the heap
  private Key[] PQ;				//Array of keys

  public multiwayMaxheap(int M, int d){
  this.d = d; 			
	PQ = new Key[M+d];
  }

  public void insert(Key v){
    PQ[N + d] = v;
    swim(N++);
  }

  public Key delMax(){
    Key max = PQ[d];
    swap(0,--N);
    sink(0);
    PQ[N + d] == null;
    return max
  }

  public void swim(int i){
    if(i > 0 && less((i-1)/d, i)) {
      swap((i-1)/d, i);
      swim((i-1)/d);
    }
  }

  public void sink(int i){
    int max = maxchild(i);
    while(i < N && less(max,i)){
      swap(i,max);
      i = max;
      max = maxchild(i);
    }
  }

  public void swap(int i, int j){
    int x = i +d, y = j +d;
    Key temp = PQ[x];
    PQ[x] = PQ[y];
    PQ[y] = temp;
  }

  public boolean less(){
    int x = i +d, y = j +d;
    return PQ[x].compareTo(PQ[y]) < 0;
  }

  public int maxchild(int i){
  int lobound = d*i+1, hibound = d*i+d;
  int max = lobound;
  for(curr = lobound; curr <= hibound; curr++){
    if(curr < N && curr > max){
      max = curr;
    }
  }
  return max;
  }
}
```

### <a name = "Resized">2. Resized Heap<a/>
**_The resized Heap is a heap with resizable capacity, which can increase the usage of memory_**
```java
private void resize(int M){
  Key[] temp = new Key[M];
  for(int i =1; i <= N; i++){
    temp[i] = PQ[i];
  }

  PQ = temp;
}

public void insert(Key v){
  N = N + 1;
  PQ[N] = v;
  if(N > PQ.length) resize(int 2*a.length)
  //swim up new item to it's right place//
  swim(N)
}

//delete a the min item from MinPQ//
public Key delmin(){
  Key Min = PQ[1];
  exchange(1,N);
  N =  N -1;
  if ((N > 0) && (N == (PQ.length ) / 4)) resize(PQ.length / 2);
  sink(1);
  //release the memory//
  PQ[N+1] = null;
  return Min
}

```

### <a name = "Index">3. Index Heap<a/>
**_Put a sequence if items with each has its own unique index into a priority queue, this heap extension enable user to save related information of an item to another list for the future retrieval and manipulation, it's something like key and value pair, we compare the value not the key!_**

* Constructor

  * `IndexMaxPQ()` : This should include three list to store items, index of items, and position of item in priority queue.

* Public method

  * `insert()` : insert a new item with a unique index to heap.

  * `delmax()` : return the index of item with highest priority.

  * `delete()` : delete a item from a heap with its information and index.

  * `change()` : change a the item of an index；（warning ! can't change a items' index to another, it's a one way change);

  * `maxindex()` : return the index of item with highest priority.

  * `max()` : return the item with highest priority.

  * `contain()` : does the index already exist? Or does the index already bound to a item ?

  * `increase()`: increase the priority of a item.

  * `decrease()`: decrease the priority of a item.
* Private method
  * `swap(int i, int j)`: exchange PQ[i] and a[j]

  * `less(int i, int j)`: return whether items[PQ[i]] is less then items[a[j]].

  * `swim(int v)`:move up PQ[v] to the correct position.

  * `sink(int v)`:move down PQ[v] to the correct position.

```java
public class IndexPQ<Key extends comparable<? super Key>>{
  private int MaxN;
  private int N = 0; //how many items in the PQ//
  private Key[] items; //list of item (related information)
  private int[] pq; //list of index of items//
  private int[] qp; //position of index in priority queue

  public IndexPQ(int M){
    this.MaxN = M;
    item = new Key[M+1];//store the information
    pq = new int[M +1];//store the index
    qp = new int[M +1];//store the position
    for(int i = 0; i<=M;i++){qp[i] = -1;}
  }

  public void insert(Key item, int i){
    if(i < 0 || i > MaxN) throw new IndexOutOfBoundsException("i <0 or i > Max");
    if(contain(i)) throw new IllegalArgumentException("Index i already exists");
    N++;
    items[i] = item;
    pq[N] = i;
    qp[i] = N;
    swim(N);
  }

  public int delmax(){
    int max = pq[1];
    swap(1,N);
    N--;
    sink(1);
    pq[N+1] = null;
    qp[pq[N+1]] = -1;
    item[pq[N+1]] = null;
    return max;
  }

  public boolean contain(int i){
    return qp[i] != -1;
  }

  public void change(int i,Key item){
    if(i < 0 || i > MaxN) throw new
    IndexOutOfBoundsException("i <0 or i > Max");
    if(!contain(i)) throw new IllegalArgumentException("Index i not exists");
    items[i] = item;
    sink(qp[i]);
    swim(qp[i]);
  }

  public void delete(int i ){
    if(i < 0 || i > MaxN) throw new
    IndexOutOfBoundsException("i <0 or i > Max");
    if(!contain(i)) throw new IllegalArgumentException("Index i not exists");
    swap(pq[i],N--);
    swim(qp[i]);
    sink(qp[i]);
    item[i] = null;
    pq[N+1] = null;
    qp[i] = -1;
  }

  public Key max(){
    if (isEmpty()) throw new NoSuchElementException("Priority queue underflow");
    return item[pq[1]];
  }

  public int maxindex(){
    if (isEmpty()) throw new NoSuchElementException("Priority queue underflow");
    return pq[1];
  }

  public void decreaseKey(int i, Key item){
    if (i < 0 || i >= maxN) throw new IndexOutOfBoundsException();
    if (!contains(i)) throw new NoSuchElementException("index i is absent");
    if (items[i].compareTo(item) <= 0)
    throw new IllegalArgumentException(
            "Calling decreaseKey() with given argument would not strictly decrease the key");
    items[i] = item;
    sink(qp[i]);
  }

  public void increaseKey(int i, Key item){
    if (i < 0 || i >= maxN) throw new IndexOutOfBoundsException();
    if (!contains(i)) throw new NoSuchElementException("index i is absent");
    if (items[i].compareTo(item) >= 0)
    throw new IllegalArgumentException(
            "Calling decreaseKey() with given argument would not strictly increase the key");
    items[i] = item;
    swim(qp[i]);
  }

  private void swap(int i, int j){
    Key temp = pq[i];
    pq[i] = pq[j];
    pq[j] = temp;
    qp[pq[i]] = i;
    qp[pq[j]] = j;
  }

  private boolean less(int i, int j){
    //i and j are the position where the index at in the heap
    return items[pq[i]].compareTo(items[pq[j]])<0);
  }

  private void swim(int v){

    while(v>1 && less(v/2, v)){
      exchange(v/2,v);
      v = v/2;
    }
  }

  private void sink(int v){
    while(2*v < N){
      int j =2*v;

      if(j < N && less(j,j+1)) j++;

      if(less(v,j)){
        exchange(v,j);
        v = j;
      }
    }
  }
}
```

<hr>

_CopyRight &copy;Newlifehaonan.com_
