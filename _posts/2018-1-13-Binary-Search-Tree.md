---
layout: post
title: "BinarySearchTree--(BST)"
date: 2018-1-6 11:30:20
tag:
- data-structure
- search
description: How to build Tree structure ? How to implement sorted symbol table using lined structure? How to implement recursive function.
comments: true
---
# BinarySearchTree(BST)

<hr />

## Appendix

* [BinarySearchTree Definition]

  * [Definition]

  * [Structure Implementation]

* [Implementation of BST]

  * [Size,Height]

  * [Get, Put]

  * [Max, Min]

  * [Floor, Ceiling]

  * [Rank, Select]

  * [Delete]

  * [Ranged Queries]

<hr />

## Binary Search Tree Definition

### Definition
**_BST is a Linked Structure composed by Node structure, which contains a Comparable Key and two referenced Node called left Node and right Node.All the key of left child node have to be less than it's parent, and all the key of right child node have to be larger than it's parent._**

### Implement.
```java
public class BST<Key extends Comparable<? super Key>,Value extends Comparable<? super Value>>{
  private Node root;
  private class Node{

    private Key key;
    private Value val;
    private Node left;
    private Node right;

    private Node(Key key, Value val){
      this.key = key;
      this.val = val;
    }  
  }
}
```
_Following Implementation are based on this class._

<hr />

## Implementation of BST

### Size & Height

**_Recursively build those two methods. For Size, Size(Root) = Size(Left) + Size(right) + 1. For Height, Height(Root) = Max(height(left),height(right)) + 1_**

**_The core idea to implement code recursively is assumed the function is correct for all cases, break down the problem to smaller part by thinking out the base case and general case_**

1. Size

```java
public int size(){
  return size(Node root);
  //supportive function//
}
private int size(Node x)
{
  if(x == null) return 0;
  return size(x.left) + size(x.right) +1;
}
```

2. Height

```java
public int height(){
  return height(Node root);
}

private int height(Node x){
  if(x == null) return 0;
  return Math.max(height(x.left), height(x.right)) +1;
}
```

### Get & Put

1. Get

**_Compare each node's key with the target key, if less, search in the left subtree, if higher, search in the right subtree until it hit the node with the same key, then return the value. If hit the null node ,return null._**

```java
public Value get(Key key){
  return get(root, key);
}

private Value get(Node x, Key key){
  if(x == null) return null;
  int cmp = x.key.compareTo(key);
  if(cmp == 0) return x.val;
  else if(cmp <=0) return get(x.right,key);
  else return get(x.left,key);
}
```

2. Put

**_If root is empty, set new node to root. If the target key is equal to some keys in the tree, update the current key. If there is no such key in current tree, put it in the tree_**

```java
public void put(Key key, Value val){
  return root = put(root,key,val)；
}

private Node put(Node x, Key key, Value val){
  if(x == null) return new Node(key,val);
  int cmp = x.key.compareTo(key);
  if(cmp == 0) x.val = val;
  if(cmp < 0) x.right = put(x.right, key,val);
  if(cmp > 0) x.left = put(x.left,key,val);
  return x;
}
```

### Max & Min

**_The node which is in the leftmost part of the tree has Min key; The node which is in the rightmost part of the tree has MAX key_**

1. Max
```java
public Value max(){
  return max(root);
}
private Value max(Node x){
  if(x.left == null) return x.val;
  return max(x.left);
}
```
2. Min

```java
public Value min(){
  return min(root);
}
private Value min(Node x){
  if(x.right == null) return x.val;
  return min(x.right);
}
```
### floor & ceiling

1. floor

**If tree is empty, return null; if target key is less than the key of current node, search at left subtree; if target key is larger than the key of current node, it MIGHT be in the right subtree.**

```java
public Key floor(Key key){
  return floor(root,key).key;
}

private Node floor(Node x, Key key){
  if(x == null) return null;
  int cmp = x.key.compareTo(key);
  if(cmp == 0) return x;
  if(cmp >0) return floor(x.left,key);
  else
  Node t = floor(x.left,key);
  if(t!=null) return t;
  else return x;
}
```

2. Ceiling

**If tree is empty, return null; if target key is higher than the key of current node, search at right subtree; if target key is less than the key of current node, it MIGHT be in the left subtree.**

```java
public Key Ceiling(Key key){
  return Ceiling(root,key).key;
}

private Node Ceiling(Node x, Key key){
  if(x == null) return null;
  int cmp = x.key.compareTo(key);
  if(cmp == 0) return x;
  if(cmp >0) return Ceiling(x.right,key);
  else
  Node t = Ceiling(x.left,key);
  if(t!=null) return t;
  else return x;
}
```

### Rank & Select

1. Rank

**_Rank return the number of keys that are less than itself. If the target key is less than the key of current node, return the size of its left subtree; If higher, return 1 + size(x.left) + rank(key,x.right)_**

```java
public int rank(Key key){
  return rank(root, key);
}

public int rank(Node x, Key key){
  if(x =null) return 0;
  int cmp = x.key.compareTo(key);
  if (cmp == 0) return size(x.left)
  else if(cmp > 0) return rank(x.left, key);
  else return rank(x.right,key) + size(x.left) +1;
}
```

2. Select

**_Return the key of node which rank is specified by user_**

```java
public Key select(int s){
  return select(root,s).key;
}

private Key select(Node x,int s){
  if(x == null) return null;
  int t = size(x.left);
  if(s > t) select(x.right, s-t-1);
  else if(s<t) select(x.left, s)
}

```

### Delete

**_Following the step to do the delete_**

* Find the node(Y) of which key needs to be deleted.

* In the right subtree of Y, find the node(Z) with min key.

* Delete Z in tree which root is Y.

* Linked Z:

  * Z.left = Y.left.

  * Z.right = Delete(Z).

```java
public void delete(Key key){
  root = delete(root, key);
}

private Node delete(Node x, Key key){
  if(x == null) return null;
  int cmp = x.key.compareTo(key);
  if(cmp < 0) delete(x.right, key);
  else if (cmp >0) delete(x.left,key);
  else
  {
  if(x.left == null) return x.right;
  if(x.right == null) return x.left
  Node Y = x;
  Z = min(Y.right);
  Z.right = Y.Delete(Z);
  Z.left = Y.left;
  }
  return x
}
```
