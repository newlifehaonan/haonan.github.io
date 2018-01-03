---
layout: post
title: "Stack and Queue"
date: 2017-12-31 11:30:20
tag:
- Java
- data-structure
description: Comparable,Comparator,Iterator in Java
comments: true
---

# Stack and Queue
**_This article collects the java implementation of Stack and Queue structure_**

<hr />

## Appendix

* [Stack](#Stack)

* [Queue](#Queue)
  * [single-way Queue](#single)
  * [double-ended Queue](#double)
  * [circular queue](#circular)

<hr />

### <a name="Stack">Stack<a />
**_Stack is an application of linked list structure, items in the stack follow the "First In First Out" pattern._**

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
#### <a name="single">1. Single-way Queue<a />

**_Queue is an application of linked list structure, items in the Queue follow the "First In Last Out" pattern._**
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

#### <a name="double">2. Double-way Queue<a />

#### <a name="circular">3. Circular Queue<a />

_CopyRight &copy;Newlifehaonan.com_
