---
layout: post
title: "Linked-List"
date: 2017-12-31 11:30:20
tag:
- Java
- data-structure
description: This article is a collections of 3 kinds of implementation of linked list includes single-way Linked List, double-way Linked List, circular Linked List.
comments: true
---
# Linked List
**_Linked list is a data structure that composed by node object, each node refers to other nodes. There are three kinds of Linked list: single-way, double-way, circular_**

<hr />

## Appendix

* [Single-way Linked List](#single)

* [Circular Linked List](#circular)

* [Double-way Linked List](#double)

<hr />

## <a name="single">Single-way Linked List(SLL)<a />

* Constructor
  * Node(): to construct node object uses for Linked List;
  * SLL: to construct SLL object;
* method
  * insert_at_front()
  * insert_at_end()
  * delete_at_front()
  * delete_at_end()
  * could have insert & delete in between if necessary!

  ```java
  public class SLL<item>{

      private class Node<item>{
        public Node();
        public item data;
        public Node next;
      }

      Node first = new Node();
      first = null;

      public SLL();

      public void insert_at_front(item a){
        Node NewNode = new Node();
        NewNode.data = a;
        if(first == null){
          NewNode.next = null;
          first = NewNode;
        }
        else{
          NewNode.next = first;
          first = NewNode;
        }
      }

      public void insert_at_end(item a){
        Node NewNode = new Node();
        NewNode.data = a;
        NewNode.next = null;
        if(first == null){
          first = NewNode;
        }
        else{
          Node temp = first;
          while(temp.next != null){
            temp = temp.next;
          }
          temp.next = NewNode;
        }
      }

      public item delete_at_front(){
        if(first ==null){throw new exception("LS is empty");}
        else{
          if(first.next == null){
            item out = first.data;
            first = null;
            return out;
          }
          else{
            item out = first.data;
            first = first.next;
            return out;
          }
        }
      }

      public item delete_at_end(){
        if(first ==null){throw new exception("LS is empty");}
        else{
          item out = first.data;
          if(first.next == null){
            out = first.data;
            first = null;
          }
          else{
            Node temp1 = first, temp2;
            while(temp1.next != null){
              temp2 = temp1;
              temp1 = temp1.next;
            }
            out = temp1.data;
            temp2.next = null;
          }
          return out;
      }
    }
  ```

## <a name="circular">Circular Linked List<a />
**_Circular Linked List is a extension of SLL. the very first node in LL will be lined to itself. There is no node that has next equals to null!_**

```java
public class CLL<item>{

    private class Node<item>{
      public Node();
      public item data;
      public Node next;
    }

    Node first = new Node();
    first = null;

    public CLL();

    public void insert_at_front(item a){
      Node NewNode = new Node();
      NewNode.data = a;
      if(first == null){
        first = NewNode;
        //!!!!!!change//
        NewNode.next = first;
      }
      else{
        Node temp = first;
        //!!!!!!change//
        while(temp.next != first){
          temp = temp.next;
        }
        //!!!!!!change//
        NewNode.next = first;
        first = NewNode;
        temp.next = first;
      }
    }

    public void insert_at_end(item a){
      Node NewNode = new Node();
      NewNode.data = a;
      if(first == null){
        first = NewNode;
        //!!!!!!change//
        NewNode.next = first;
      }
      else{
        Node temp = first;
        //!!!!!!change//
        while(temp.next != first){
          temp = temp.next;
        }
        temp.next = NewNode;
        NewNode.next = first;
      }
    }

    public item delete_at_front(){
      if(first ==null){throw new exception("LS is empty");}
      else{
        item out=first.data;
        //!!!!!!change//
        if(first.next == first){
          first = null;
        }
        else{
          first = first.next;
        }
        return out;
      }
    }

    public item delete_at_end(){
      if(first ==null){throw new exception("LS is empty");}
      else{
        item out;
        //!!!!!!change//
        if(first.next == first){
          out = first.data;
          first = null;
        }
        else{
          Node temp1 = first, temp2;
          //!!!!!!change//
          while(temp1.next != first){
            temp2 = temp1;
            temp1 = temp1.next;
          }
          out = temp1.data;
          temp2.next = first; //!!!!!!change//
        }
        return out;
    }

  }
```

## <a name="double">Double-way Linked List<a />
**_Double linked list is a sequence of elements in which every element has links to its previous element and next element in the sequence._**

```java
public class DLL<item>{

    private class Node<item>{
      public Node();
      public item data;
      public Node next;
      public Node previous;
    }

    Node first = new Node();
    first = null;

    public DLL();

    public void insert_at_front(item a){
      Node NewNode = new Node();
      NewNode.data = a;
      //!!!!!change//
      NewNode.previous = null;
      if(first == null){
        NewNode.next = null;
        first = NewNode;
      }
      else{
        NewNode.next = first;
        first = NewNode;
      }
    }

    public void insert_at_end(item a){
      Node NewNode = new Node();
      NewNode.data = a;
      NewNode.next = null;
      if(first == null){
        NewNode.previous = null;
        first = NewNode;
      }
      else{
        Node temp = first;
        while(temp.next != null){
          temp = temp.next;
        }
        temp.next = NewNode;
        //!!!!!!!change//
        NewNode.previous = temp;
      }
    }

    public item delete_at_front(){
      if(first ==null){throw new exception("LS is empty");}
      else{
        if(first.next == first.previous){
          item out = first.data;
          first = null;
          return out;
        }
        else{
          item out = first.data;
          first = first.next;
          first.previous = null;
          return out;
        }
      }
    }

    public item delete_at_end(){
      if(first ==null){throw new exception("LS is empty");}
      else{
        item out = first.data;
        if(first.next == first.previous){
          out = first.data;
          first = null;
        }
        else{
          Node temp1 = first;
          while(temp1.next != null){
            temp1 = temp1.next;
          }
          out = temp1.data;
          temp1.previous.next = null;
        }
        return out;
    }
  }
```

_CopyRight &copy;Newlifehaonan.com_
