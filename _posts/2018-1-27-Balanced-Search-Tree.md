---
layout: post
title: "Balanced Search Tree"
date: 2018-1-6 11:30:20
tag:
- data-structure
- search
description: 2-3 Tree, Red-Black Tree
comments: true
---
# Balanced Search Tree

**Compared to Binary search tree, Balanced Search Tree will keep each branch to be the same height no matter how you insert or delete the item so that it keeps the search method to be logarithmic.**

<hr />

# Appendix

* [2-3 Tree](#23)

* [Red-Black Tree](#RB)

<hr >

# <a name = "23">2-3 Tree<a/>

**2-3 Tree are composed by 2-Node and 3 Node structure, we apply promote and demote algorithm on insert and delete function under different scenario. The strategy are promote the 2-Node to be 3-Node, promote 3-Node to be temporary 4-Node and split the 4-Node in proper way.**

## Node Structure

```java
public class Node<Key, Value>{
  private int type = 2;
  private Key key;
  private Value val;
  public 2_Node<Key,Value>(Key key, Value val){
    this.key = key;
    this.value = val;
    Node left;
    Node right;
  }
}

public class Node<Key, Value>{
  private int type = 3;
  private Key key1, key2;
  private Value val1, val2;
  public 3_Node<Key,Value>(Key key1, Key key2, Value val1,Value val2){
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

![insert2]({{'assets/images/insert2.png' | relative_url}}){: .center-image };

**Assume key1 save the smaller key and key2 save the larger key in a 3 node, there are two conditions that need to be considered, first the insert key is smaller than current key, second is larger.**

```java
public boolean is2node(Node x){
  return x.type == 2;
}

public boolean is2node(Node x){
  return x.type == 3;
}

public void 2to3(Key key, Value val){
  root = 2to3(root, key, val);
}

private Node 2to3(Node x, Key key, Value val){
  int cmp = x.key.compareTo(key);
  if(cmp <0) {
    Node t = new Node(key1 = x.key, key2 = key, val1 = x.val, val2 = val);
    //new node's left node must be x's left node, but mid and right remain unknown.
    t.left = x.left;
    //t.mid ??;
    //t.right ??;
    x = t;
  }
  else if (cmp >0) {
    t = new Node(key1 = key, key2 = x.key, val1 = val, val2 = x.val);
    //new node's right node must be x's left node, but mid and left remain unknown.
    t.right = x.right;
    //t.mid ??;
    //t.left ??;
    x = t;
  }
  else x.val = val;
  return x;
}
```

## Insert Scenario 2: Turing temporary 4_Node to a tree in which all nodes are 2-node.

![4to2]({{'assets/images/4to2.png' | relative_url}}){: .center-image };

**Promote mid key and split the 4_Node**

```java
public void 4to2(Key key, Value val){
  root = 4to2(root, key, val);
}

private Node 4to2(Node x, Key key, Value val){
  int cmp1 = key.compareTo(x.key1);
  int cmp2 = key.compareTo(x.key2);
  if(cmp1 < 0){
    Node t = new Node(x.key1, x.val1);
    t.left = new Node(key, val);
    t.right = new Node(x.key2, x.val2);
  }
  else if(cmp1 >0 && cmp2 <0){
    Node t = new Node(key, val);
    t.left = new Node(x.key1, x.val1);
    t.right = new Node(x.key2, x.val2);
  }
  else if(cmp2 >0){
    Node t = new Node(x.key2, x.val2);
    t.left = new Node(x.key1, x.val1);
    t.right = new Node(key, val);
  }
  else if(cmp1 == 0){
    x.val1 = val
  }
  else x.val2 = val;
  return x;
}
```

## Insert Scenario 3: Insert to a 3_Node whose parent is a 2_Node

![parents2]({{'assets/images/parents2.png' | relative_url}}){: .center-image };

```java

```

## Insert Scenario 4: Insert to a 3_Node whose parent is a 3_Node

![parents3]({{'assets/images/parents3.png' | relative_url}}){: .center-image };

```java

```

<hr />

# <a name = "RB">Red-Black Tree<a/>

**we represent 3-nodes as two 2-nodes connected by a single red link that leans left (one of the 2-nodes is the left child of the other).**

## Definition and Convention

* Red node: if the link from its parent is red then the node is red.

* Black node: if the link from its parent is black then the node is black.

* leans left: red links alway be the left links.

* no double red links.

* the tree has perfect black balance which means every path from the root to a null link has the same number of black links.

* new come node are red by default.

* root node are always black.

## Node defination

```java
public class Node<Key, Value>{
  private boolean color;
  private Key key;
  private Value val;
  public 2_Node<Key,Value>(Key key, Value val, boolean type){
    this.key = key;
    this.value = val;
    this.type = type;
    Node left;
    Node right;
  }
}
```

## Rotation
**To keep the red link leans left, We need to write two rotate functions.**

![rotateleft]({{'assets/images/rotateleft.png' | relative_url}}){: .center-image }

```java
public void rotateleft(Node h){
  Node x = h.right;
  h.right = x.left;
  x.left = h;
  x.color = h.color;
  h.color = RED;
  return x;
}

```

![rotateright]({{'assets/images/rotateright.png' | relative_url}}){: .center-image }

```java
public void rotateright(Node h){
  Node x = h.left;
  h.left = x.right;
  x.right = h;
  x.color = h.color;
  h.color = RED;
  return x;
}
```

## Insert into a single 2-node.

![RB2]({{'assets/images/RB2.png' | relative_url}}){: .center-image }

## Insert into a 3-node.

![RB3]({{'assets/images/RB3.png' | relative_url}}){: .center-image }

```java
private void flipColors(Node h){
  h.color = RED;
  h.left.color = BLACK;
  h.right.color = BLACK;
}

public void put(Key key, Value val){
  root = put(root, key, val);
  root.color = BLACK;
}

private Node put(Node h, Key key, Value val){
  if (h == null)  // Do standard insert, with red link to parent.
  return new Node(key, val, 1, RED);
  int cmp = key.compareTo(h.key);
  if      (cmp < 0) h.left  = put(h.left,  key, val);
  else if (cmp > 0) h.right = put(h.right, key, val);
  else h.val = val;
  // make the tree balance//
  if (h.right.color == RED && h.left.color == BLACK)    h = rotateLeft(h);
  if (h.left.color == RED && h.left.left.color == RED) h = rotateRight(h);
  if (h.left.color == RED && h.right.color == RED)     flipColors(h);
  return h;
}
```

## DeleteMin

## DeleteMax

## Delete


<hr />
