---
layout: post
title: "Symbol Table Implementation"
date: 2018-1-6 11:30:20
tag:
- data-structure
- search
description: This article is a collection of search algorithm in symbol table using different data-structure.
comments: true
---

# Symbol Table(ST)

<hr />

## Appendix

* [Definition and Conventions](#Definition)

* [Unordered Linked List ST](#Sequential)

* [Ordered parallel array ST](#Binary)

<hr />

## <a name ="Definition">Definition<a />

**_A symbol table is a data structure for key-value pairs that supports two operations: insert (put) a new pair into the table and search for (get) the value associated with a given key.There are several conventions towards Key and Value when Implement symbol table_**

* **Generics**

  _Key and Value are not limited to primitive datatype. It can be any Comparable object_

* **Key Uniqueness**

  _1. Key can't be NULL._

  _2. Key must be unique, one key one value. but the value do not need to be unique._

* **Null conventions**

  _1. When key is not in table, get(key) returns NULL._

  _2. Put(key, null) is a lazy delete method , it will delete the value which associated with the key.Eager deletion will delete the key and its value immediately_

* **Iterable and Comparable and equality**

  _1. `keys()` will return a iterator object for traversal._

  _2. Key must be Comparable_

  _3. a.equals(b) returns true if and only if a and b have the same length and are identical in each character position._

<hr />

## <a name="Sequential">Unordered Linked List ST<a />

**_The Node uses to construct linked list has three value, one for key, one for value, and one for referring next node._**

### API
* Constructors

  * For Linked list: `Node()` and `ST()`

  * For parallel array `ST()`

* Methods
  * `size()`

  * `isEmpty()`

  * `contain(Key a)`

  * `put(Key key, Value value)`

  * `get(Key key)`

  * `Delete(Key key)`

  * `rank(Key key)` return how many keys that are less than key.

### implementation

```java
public class SequentialST<Key extends Comparable<? super Key>,Value extends Comparable<? super Value>>{
  private Node first;

  /*Constructor*/
  private class Node{
    private Key key;
    private Value val;
    private Node next;

    public Node(Key key, Value val){
      this.key = key;
      this.val = val;
      this.next= next;
    }
  }

  public SequentialST(){};

  /*Size, isEmpty,contain method*/
  public int size(){
    return size(first);
  }

  private int size(Node x){
    if(x == null) return 0;
    return size(x.next) +1;
  }

  public boolean isEmpty(){
    return first == null;
  }

  public boolean contain(Key key){
    return get(key) != null;
  }

  /*Get, put, delete and rank method ( recursively)*/
  public Value get(Key key){
    Value val = get(first,key);
  }

  private Value get(Node x, Key key){
    if(x == null) return null;
    if(x.key.equals(key)) return x.value;
    return get(x.next, key);
  }

  public void put(Key key, Value value){
    first = put(Node x, Key key, Value value);
  }

  private Node put(Node x, Key key, Value value){
    if(x == null) return new Node(key, value);
    int cmp = x.key.compareTo(key);
    if(cmp == 0) x.value = value;
    else         x.next = put(x.next, key, value);
    return x;
  }

  public void delete(Key key){
    first = delete(first, key);
  }

  private Node delete(Node x, Key key){
    if(x == null) return null;
    if(x.key.equals(key)) {return x.next}
    x.next = delete(x.next,key);
    return x;
  }

  public int rank(Key key) {
    return rank (first,key);
  }

  private int rank (Node x, Key key) {
    if(x ==null) return 0;
    int cmp = x.key.compareTo(key);
    if(cmp>=0) return rank(x.next,key);
    else		   return rank(x.next,key) +1;
  }
}
```

<hr />

## <a name="Binary">Ordered parallel array ST<a />
**_Use Binary search to get key and insert new key into ordered array_**
### API

* Constructors

  * For Linked list: `Node()` and `ST()`

  * For parallel array `ST()`

* Methods
  * `size()`

  * `isEmpty()`

  * `contain(Key a)`

  * `put(Key key, Value value)`

  * `get(Key key)`

  * `Delete(Key key)`

  * `min()`: return smallest key.

  * `max()`: return largest key.

  * `rank(Key key)`: return the position of key in ST.

  * `select(int a)`: return key information at position a in ST.

  * `floor(Key key)`: largest key less than or equal to key.

  * `ceiling(Key key)`: smallest key greater than or equal to key.

### implementation

```java
public class BinarySearchST<Key extends Comparable<? super Key>,Value extends Comparable<? super Value>>{
  private Key[] keys;
  private Value[] vals;
  private int N;
  public BinarySearchST(int capacity){
    keys = new Key[capacity];
    vals = new Value[capacity];
  }

  public int size(){
    return N;
  }

  public boolean isEmpty(){
    return N==0;
  }

  public boolean contain(Key key){
    return get(key != null);
  }
  public int rank(Key key){
    rank(key,0,N-1);
  }
  private int rank(Key key, int lo, int hi){
    if (hi < lo) return lo;
    int mid = lo + (hi - lo) / 2;
    int cmp = key.compareTo(keys[mid]);
    if      (cmp < 0)
         return rank(key, lo, mid-1);
    else if (cmp > 0)
         return rank(key, mid+1, hi);
    else return mid;
  }

  public Value get(Key key){
    if (isEmpty()) return null;
    int i = rank(key);
    if (i < N && keys[i].compareTo(key) == 0) return vals[i];
    else                                      return null;
  }

  private void resize(int size) {
		Key[] tempkeys = (Key[])(new Comparable[size]);
		Value[] tempvals = (Value[])(new Object[size]);
		for(int i =0; i < keys.length; i++) {
			tempkeys[i] = keys[i];
			tempvals[i] = vals[i];
		}
		keys = tempkeys;
		vals = tempvals;
	}

  public void put(Key key, Value val){
    int i = rank(key);
    if (i < N && keys[i].compareTo(key) == 0){  vals[i] = val; return;}
    if (N == keys.length) resize(2*keys.length);
    for (int j = N; j > i; j--){keys[j] = keys[j-1]; vals[j] = vals[j-1];}
    keys[i] = key; vals[i] = val;
    N++;
    }

  public void delele(Key key, Value val){
    if (isEmpty()) return;
    int i = rank(key);
    if (i < N && keys[i].compareTo(key) == 0){
      for (int j = i; j < N-1; j++){
        keys[i] = keys[i+1];
        vals[i] = vals[i+1];
      }
      N--;
      keys[N] = null;  // to avoid loitering
      vals[N] = null;
      if (N > 0 && N == keys.length/4) resize(keys.length/2);
    }
    else return;
  }

  public Key min() {

    if (isEmpty()) return null;
    return keys[0];
  }

  public Key max() {
    if (isEmpty()) return null;
    return keys[N-1];
  }

  public Key select(int k) {
    if (k < 0 || k >= N) return null;
    return keys[k];
  }

  public Key floor(K key) {
    int i = rank(key);
    if (i < N && key.compareTo(keys[i]) == 0) return keys[i];
    if (i == 0) return null;
    else return keys[i-1];
  }

  public Key ceiling(K key) {
    int i = rank(key);
    if (i == N) return null;
    else return keys[i];
  }
}
```


_CopyRight &copy;Newlifehaonan.com_
