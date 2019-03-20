---
layout: post
title: "Tries structure & TST"
date: 2018-3-5 15:00:00
tag:
- data-structure
- search
description: Introduction of Tries structure and TST tree
comments: true
---

# Tries
<hr />

## Appendix

<hr />

* Tries structure

  * Basic properties

  * size

  * search (get) & insert (put)

  * collecting keys

  * delete

* TST

  * Basic properties

  * search (get) & insert (put)

<hr />

# Tries structure
**a data structure built from the characters of the string keys that allows us to use the characters of the search key to guide the search.**
## Basic properties

* tries are data structures composed of nodes that contain links that are either null or references to other nodes

* each node has R links, where R is the alphabet size

* Root has no node pointed to it.

* Each node also has a corresponding value, which may be null or the value associated with one of the string keys in the symbol table.

* SEARCH HIT:

  * String end & the last character of the string has a value.

* SEARCH MISS:

  * String end & last character has null value.

  * null link.

* PUT

  * when encounter a null link, create that link.

  * when reaches the last character, update the correspond value.

## Size
**There is a list containing all the referenced Node if has, letter are just index**
```java
public int size()
{ return size(root); }

public int size(Node x)
{
  if (x==null) return 0;
  int cnt =0;
  if(x.val != null) cnt++;
  for(char c = 0; c<R; c++){
    cnt += size(next[c]);
  }

  return cnt;
}
```
## put and get

### Recursive version
```java
public class TrieST<Value>
 {
    private static int R = 256; // radix
    private Node root;          // root of trie
    private static class Node
    {
       private Object val;
       private Node[] next = new Node[R];
    }

    public Value get(String key)
    {
       Node x = get(root, key, 0);
       //if tries are empty， return null
       if (x == null) return null;
       // if search hit, return the value
       return (Value) x.val;
     }

    private Node get(Node x, String key, int d)
    {  // Return value associated with key in the subtrie rooted at x.
       if (x == null) return null;
       //if reaches the end of the string
       if (d == key.length()) return x;
       char c = key.charAt(d); // Use dth key char to identify subtrie.
       return get(x.next[c], key, d+1);
    }

    public void put(String key, Value val)
    {  root = put(root, key, val, 0);  }

    private Node put(Node x, String key, Value val, int d)
    {  // Change value associated with key if in subtrie rooted at x.
       if (x == null) x = new Node();
       if (d == key.length()) {  x.val = val; return x; }
       char c = key.charAt(d); // Use dth key char to identify subtrie.
       x.next[c] = put(x.next[c], key, val, d+1);
       return x;
     }
}
```

### Iterative version
```java
public class Trie {

    // Alphabet size (# of symbols)
    private static final int R = 26;
    private static Node root;
    // trie node


    static void insert(String key, Value val)
    {
        char c;
        int length = key.length();
        int d;

        Node x = root;

        for (d = 0; d < length; d++)
        {
            c = key.charAt(d)
            if (x.next[c] == null)
                x.next[c] = new Node();

            x = x.next[c];
        }
        x.val = val;
        return;
    }

    // Returns true if key presents in trie, else false
    static Value search(String key)
    {
        int d;
        int length = key.length();
        char c;
        Node x = root;

        for (d = 0; d < length; d++)
        {
            c = key.charAt(d);

            if (x.next[c] == null)
                return null;

            x = x.next[c];
        }

        return (Value) x.value;
    }
}
```
## Collecting keys

```java


```

## deletion

<hr />

# TST

## Basic properties

**To help us avoid the excessive space cost associated with R-way tries, we now consider an alternative representation: the ternary search trie (TST).**

* each node has a character, three links, and a value.

* The three links correspond to keys whose current characters are less than, equal to, or greater than the node’s character.

* SEARCH

  * Search hit: we terminate with a search hit if the node where the search ends has a non-null value.

  * Search miss: We terminate with a search miss if we encounter a null link or if the node where the search ends has a null value.

* PUT

  * we search, then add new nodes for the characters in the tail of the key

## Get and put

### recursive version
```java
public class TST<Value>
{
  private Node root;// root of trie
  private class Node
  {
    char c;// character
    Node left, mid, right;// left, middle, and right subtries
    Value val;// value associated with string
  }

  public Value get(String key)
  {
    Node x = get(root, key, 0);
    if (x == null) return null;
    return (Value) x.val;
  }

  private Node get(Node x, String key, int d)
  {
    if (x == null) return null;
    char c = key.charAt(d);
    if      (c < x.c) return get(x.left,  key, d);
    else if (c > x.c) return get(x.right, key, d);
    else if (d < key.length() - 1) return get(x.mid, key, d+1);
    else return x;
  }

  public void put(String key, Value val)
  {  
    root = put(root, key, val, 0);
    return get(x.mid, key, d+1);
  }

  private Node put(Node x, String key, Value val, int d)
  {
    char c = key.charAt(d);
    if (x == null) { x = new Node(); x.c = c; }
    if      (c < x.c) x.left  = put(x.left,  key, val, d);
    else if (c > x.c) x.right = put(x.right, key, val, d);
    else if (d < key.length() - 1) x.mid   = put(x.mid, key, val, d+1);
    else x.val = val;
    return x;
  }
}
```
### Iterative version

```java
public class TST<Value>
{
  private Node root;// root of trie
  private class Node
  {
    char c;// character
    Node left, mid, right;// left, middle, and right subtries
    Value val;// value associated with string
  }

  public Value get(String key)
  {
    Node x = root;
    int d =0;
    int length = key.length;
    char c;
    if( x == null) return null;    
    while(d<length){
      c = key.charAt(d);
      int cmp = x.c.compareTo(c);
      if(cmp == 0) {
        x=x.mid;
        d++;
      }
      else if(cmp <0) x = x.right;
      else x=x.left;
    }
		return x.val;  //Todo 1
  }

  private Node put(String key, Value val)
  {
    Node x = root;
    int d;
    int length = key.length;
    char c;
    if(x == null) {
      for(d = 0; d <length; d++){
        c = key.charAt(d);
        x = x.mid = c;
      }
    }

    while(x < length) {
        c = key.charAt(d);
        int cmp = x.c.compareTo(c);
        if(cmp == 0) {
          x=x.mid;
          d++;
        }
        else if(cmp <0) x = x.right;
        else x=x.left;
    }
    x.val = val;
    return x;
  }
}
