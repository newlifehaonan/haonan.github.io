---
layout: post
title: "Union & Find"
date: 2017-12-31 11:30:20
tag:
- data-structure
description: Includes 4 union find algorithms
comments: true
---

# Union Find

**_Union Find algorithm uses array structure to link components together_**

<hr />

## Appendix

* [Term Definition](#Definition)

* [Quick find](#QF)

* [Quick Union](#QU)

* [Weighted Union](#WU)

* [Weighted Union with Path Compression](#PC)


<hr />

## <a name ="Definition">Term Definition<a/>

* Component: If the root of node p and q are same, they belongs to same component.

* Node depth: The number of edges between node and root.

* Tree height: The number of edges in the longest path in the tree.

<hr />

## <a name ="QF">Quick Find<a/>

**_In quick find, if we want to link p and q, we directly change all items whose component is also p to q._**

```java
public class quickfind{
  public Item[] id;
  public int count;
  public quickunion(Item[] id){
    this.id = id;
    this.count = id.length;
  }

  public static int find(int i){
    return id[i];
  }

  public static void union(int p, int q){
    int pid = find(p);
    int qid = find(q);

    if(qid == qid) return;
    for(int i = 0 ; i < id.length; i++){
      if(pid == id[i]) id[i] = qid;
    }
    count--;
  }
}
```
<hr />

## <a name ="QU">Quick Union<a/>

**_In quick union, if we want to link p and q, Generally we always connect p to q, Specifically we always connect p's root to q's root_**

```java
public class quickunion<Item>{
  public Item[] id;
  public int count;
  public quickunion(Item[] id){
    this.id = id;
    this.count = id.length;
  }

  public static int find(int i){
    while(id[i] != i){
      i = id[i];
    }
    return i;
  }

  public static void union(int p, int q){
    int pid = find(p);
    int qid = find(q);
    if(pid == qid) return;
    id[pid] = qid;
    count--;
  }
}

```
<hr />

## <a name ="WU">Weighted Union<a/>

**_Compare to Quick Union, we always link p's root to q's root no matter how big is the tree which p belongs to. The optimization is linking small tree to large tree_**

```java
public class Weightedunion{
  public Item[] id;
  public int count;
  public int size[];
  public quickunion(Item[] id){
    this.id = id;
    this.count = id.length;
    for(int i = 0; i < id.length; i++){
      this.size[i] = 1;
    }
  }

  public static int find(int i){
    while(id[i] != i){
      i = id[i];
    }
    return i;
  }

  public static void union(int p, int q){
    int pid = find(p);
    int qid = find(q);
    if(pid == qid) return;
    if(size[p] < size[q]){
      id[pid] = qid;
      size[q] = size[q]+size[p];
    }else{
      id[qid] = pid;
      size[p] = size[p]+size[q];
    }
    count--;
  }
}
```
<hr />

## <a name ="PC">Weighted Union with Path Compression<a/>

**_Trying to make the tree to be more balance, which decrease the hight of the tree.The method is trying to linked all the nodes along the path to the root directly to the root._**

**_The goal of find is to find the root of a node, but along the finding journey, we optimize the algorithm by compress the path_**

```java
public class Weightedunion{
  public Item[] id;
  public int count;
  public int size[];
  public quickunion(Item[] id){
    this.id = id;
    this.count = id.length;
    for(int i = 0; i < id.length; i++){
      this.size[i] = 1;
    }
  }

  public static int find(int i){
    int k, j, r;
    r = i; //r save the root of i
    while(id[r] != r){
      r = id[r];
    }

    k = i; //k is the count variable during iteration
    while(id[k] != k){
      j = id[k] //j save the parent of k
      id[k] = r //make k's parent directly to r(root)
      k = j //update the count variable, I mean start to deal with next node
    }

    return r;

  }

  public static void union(int p, int q){
    int pid = find(p);
    int qid = find(q);
    if(pid == qid) return;
    if(size[p] < size[q]){
      id[pid] = qid;
      size[q] = size[q]+size[p];
    }else{
      id[qid] = pid;
      size[p] = size[p]+size[q];
    }
    count--;
  }
}
```
