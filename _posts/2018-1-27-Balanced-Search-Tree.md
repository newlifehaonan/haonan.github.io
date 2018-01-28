---
layout: post
title: "Balanced Search Tree"
date: 2018-1-6 11:30:20
tag:
- data-structure
- search
description: 2-3 Tree, Red-Black Tree, AVL Tree, B Tree
comments: true
---
# Balanced Search Tree

**Compared to Binary search tree, Balanced Search Tree will keep each branch to be the same height no matter how you insert or delete the item so that it keeps the search method to be logarithmic.**

<hr />

# Appendix

* [2-3 Tree](#23)

* [Red-Black Tree](#RB)

* [AVL Tree](#AVL)

* [B Tree](#BT)

<hr >

# 2-3 Tree

**2-3 Tree are composed by 2-Node and 3 Node structure, we apply promote and demote algorithm on insert and delete function under different scenario. The strategy are promote the 2-Node to be 3-Node, promote 3-Node to be temporary 4-Node and split the 4-Node in proper way.**

## Node Structure

```java
public class 2_Node<Key, Value>{
  private Key key;
  private Value val;
  public 2_Node<Key,Value>(Key key, Value val){
    this.key = key;
    this.value = val;
    Node left;
    Node right;
  }
}

public class 3_Node<Key, Value>{
  private Key key1, key2;
  private Value val1, val2;
  public 2_Node<Key,Value>(Key key1, Key key2, Value val1,Value val2){
    this.key1 = key1;
    this.value1 = val1;
    this.key2 = key2;
    this.value2 = val2;
    Node left;
    Node mid;
    Node right;
  }
}
```

## Insert Scenario 1: Insert into 2-node.

![insert2]({{'assets/images/insert2.png' | relative_url}}){: .center-image }*(°0°)*;
