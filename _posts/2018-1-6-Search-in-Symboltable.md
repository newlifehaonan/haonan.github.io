---
layout: post
title: "Symbol table implementation"
date: 2017-12-31 11:30:20
tag:
- data-structure
- Search
description: This article is a collection of search algorithm in symbol table using different data-structure.
comments: true
---

# Symbol Table

<hr />

## Appendix

* [Definition and Conventions](#Definition)

* [API](#API)

* [Implement `Put` and `Get` method using different search method](#Implement)

  * [Sequential Search(Linked List)](#Sequential)

  * [Binary Search(Ordered, parallel array)](#Binary)

<hr />

## Definition

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

## API

* Constructors

  * For Linked list: `Node()` and `ST()`

  * For parallel array `ST()`

* Methods

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

## Implementation

### Sequential Search(Linked List)

### Binary Search(parallel array)
