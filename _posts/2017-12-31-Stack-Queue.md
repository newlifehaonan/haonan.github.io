---
layout: post
title: "Stack and Queue"
date: 2017-12-31 11:30:20
tag:
- Java
- data-structure
description: This article is a collection of both array and lined list implementation of Stack and Queue.
comments: true
---

# Stack and Queue
**_This article collects the java implementation of Stack and Queue structure_**

<hr />

## Appendix

* [Stack](#Stack)
  * [Array implementation](#arraystack)
  * [Linked List Version](#llstack)
* [Queue](#Queue)
  * [single-way Queue(Array)](#swqarray)
  * [single-way Queue(Linked List)](#swqLL)
  * [double-ended Queue](#double)

<hr />

### <a name="Stack">Stack<a />
**_Stack is an application of linear data structure, items in the stack follow the "First In First Out" pattern._**
#### <a name="arraystack">1. Array implementation<a />

#### <a name="llstack">2. Linked List Version<a />

* Constructor
  * `node()`: object composes stack;
  * `stack()`: FIFO lined list;
* method
  * `push(item A)`: add item A on stack;
  * `pop()`: return the item on the top of stack
  * `isEmpty()`: return true if N = 0;

```java
public class Stack<item extends Comparable<? super item>>{

  private class node<item extends Comparable<? super item>>{
    public node() {};
    public item item;
    public node next;
  }

  private node first;
  private int N;

  public Stack(){
    this.first = null;
    this.N =0;
  }

  public boolean isEmpty(){
    return N == 0;
  }

  public void push(item A){
    node oldfirst = first;
    first = new node();
    first.item = A;
    first.next = oldfirst;
    N++;
  }

  public item pop(){
    item out = first.item;
    first = first.next;
    N--;
    return out;
  }

}
```

### <a name="Queue">Queue<a />
**_Queue is an application of linear data structure, items in the Queue follow the "First In Last Out" pattern._**
#### <a name="single">1. Single-way Queue(Array)<a />
**_Create a fixed sized array as a container of Queue and following the FILO pattern._**

```java
public class Queue<item>{
  private int N;
  private item[] Q;
  private int front = rear = -1;

  public Queue(int size){
    Q = new item[size];
  }

  public int capacity(){
    return Q.length;
  }

  public boolean isEmpty(){
    return front == rear == -1;
  }

  public boolean isFull(){
    return (rear == size-1 && front == 0) ||(front == rear+1);
  }

  public void enqueue(item a){
    if(isFull()) throw new IndexOutOfRange("Queue is Full !!");
    if(rear == size-1 && front != 0){
      rear = -1;
    }
    Q[++rear] = a;
    if(front == -1){
      front = 0;
    }
  }

  public item dequeue(){
    if(isEmpty()) throw new IndexOutOfRange("Queue is empty !!");
    item out = Q[front];
    if(front == size){
      front = 0;
    }
    if(front-1 == rear){
      front = rear = -1;
    }
  }
}

```

#### <a name="single">2. Single-way Queue(Linked List)<a />

* Constructor
  * `node()`: object composes stack;
  * `queue()`: FIFO lined list;
* method
  * `enqueue(item A)`: put item in line;
  * `dequeue()`: return the LAST item in queue;
  * `isEmpty()`: return true if N = 0;

```java
public class queue<item extends Comparable<? super item>>{
  private class node<item extends Comparable<? super item>>{
    public node() {};
    public item item;
    public node next;
  }

  private node first;
  private node last;
  private int N;

  public queue(){
    this.N =0;
  }

  public boolean isEmpty(){
    return N == 0;
  }


  public void enqueue(item A){
    Node oldlast = last;
    last = new node();
    last.item = A;
    last.next = null;
    //very important !!!!!! it will give the value to the first node//
    if(isEmpty()) {first = last;}
    else          {oldlast.next = last;}
    N++;
  }

  public item dequeue(){
    item out = first.item;
    first = first.next;
    if(isEmpty()){last = null;}
    N--;
    return out;
  }
}
```

#### <a name="double">3. Double-way Queue<a />


_CopyRight &copy;Newlifehaonan.com_
