---
layout: post
title: "Hash Table"
date: 2018-1-31 11:30:20
tag:
- data-structure
- search
description: Hash Function, Collision resolution, Separate Chain, Open address
comments: true
---
# Hash Table

<hr />

## Appendix

* Hash Function
  * Definition

  * Hashing Integer

  * Hashing String

  * converting into table index

  * user defined hash

* Collision resolution

  * Separate Chaining

  * linear probing

<hr />

## Hash Function

<hr />

### Definition
**Basic idea of Hashing is put key into a function, the function turns the key into table index, ideally each key has its own index, which refer to a address in memory so that we can easily get the key in O(1) time complexity. Here are some important convention toward hashing**

* Good Hash function will distribute the key uniformly. which mean no big cluster or collision.

* Hashing is a good example of time and memory trade off, memory is cheap so try as much as you can to distribute the key uniquely through hashing.

* Key with special meaning need to be carefully considered.


### Hash Integer

* Modular hashing
**we choose the array size M to be prime and, for any positive integer key k, compute the remainder when dividing k by M. This function is very easy to compute (k % M, in Java) and is effective in dispersing the keys evenly between 0 and M – 1.**

### Hash String
**A classic algorithm known as Horner’s method gets the job done with N multiplications, additions, and modulus operations.**

```Java
public int GetHashCode(string str)
{
    char[] s = str.ToCharArray();
    int hash = 0;
    for (int i = 0; i < s.Length; i++)
    {
        hash = s[i] + (31 * hash);
    }
    return hash;
}
```
**h = s[0] · 31^(L–1) + … + s[L – 3] · 31^2 + s[L – 2] · 31^1 + s[L – 1] · 31^0.**

**In order to save computing time, we don't need to add every number's hash, we can randomly choose or define string step**

```java
public int GetHashCode(string str)
{
    char[] s = str.ToCharArray();
    int hash = 0;
    int skip = Math.Max(1, s.Length / 8);
    for (int i = 0; i < s.Length; i+=step)
    {
        hash = s[i] + (31 * hash);
    }
    return hash;
}
```

### converting to table index
```java
private int hash(Key x)
{  
  return (x.hashCode() & 0x7fffffff) % M;
}
```

### user defined hash( for struct)
**In user defined object, simply adding each primitive data type's hash together to generate reference data type's hash code**
```Java
public class Transaction
{
  ...
  private final String who;
  private final Date when;
  private final double amount;
  public int hashCode()
  {
    int hash = 17;
    hash = 31 * hash + who.hashCode();
    hash = 31 * hash + when.hashCode();
    hash = 31 * hash
             + ((Double) amount).hashCode();
    return hash;
  }
  ...
}
```
<hr />

## Collision resolution

<hr />

### Separate Chaining
![sc]({{'assets/images/sc.png' | relative_url}}){: .center-image };

**Create a array with each address reference to a linked list (symbol table), if collision happens add them to the symbol table**

```Java
public class SeperateChainingHashSet<TKey, TValue>
{
    private int M;
    private SequentSearchSymbolTable<TKey, TValue>[] st;//

    public SeperateChainingHashSet()
    { this(997);}

    public SeperateChainingHashSet(int m)
    {
        this.M = m;
        st = new SequentSearchSymbolTable<TKey, TValue>[m];
        for (int i = 0; i < m; i++)
        {
          st[i] = new SequentSearchSymbolTable<TKey, TValue>();
        }
    }

    private int hash(TKey key)
    {
        return (key.HashCode() & 0x7fffffff) % M;
    }

    public override TValue Get(TKey key)
    {
        return st[hash(key)].Get(key);
    }

    public override void Put(TKey key, TValue value)
    {
        st[hash(key)].Put(key, value);
    }
}
```

### linear probing
![lp]({{'assets/images/lp.png' | relative_url}}){: .center-image };

**The simplest open-addressing method is called linear probing: when there is a colli- sion (when we hash to a table index that is already occupied with a key different from the search key), then we just check the next entry in the table (by incrementing the index). Linear probing is characterized by identifying three possible outcomes:**
* Key equal to search key: search hit
* Empty position (null key at indexed position): search miss
* Key not equal to search key: try next entry

```java
public class LinearProbingHashSet<TKey, TValue>
{
    private int N;
    private int M = 16;
    private TKey[] keys;
    private TValue[] values;

    public LinearProbingHashSet()
    {
        keys = new TKey[M];
        values = new TValue[M];
    }

    private int hash(TKey key)
    {
        return (key.GetHashCode() & 0xFFFFFFF) % M;
    }

    public TValue Get(TKey key)
    {
        for (int i = hash(key); keys[i] != null; i = (i + 1) % M)
        {
            if (key.Equals(keys[i])) { return values[i]; }
        }
        return null;
    }

    public void Put(TKey key, TValue value)
    {
        int hashCode = hash(key);
        for (int i = hashCode; keys[i] != null; i = (i + 1) % M)
        {
            if (keys[i].Equals(key))
            {
                values[i] = value;
                return;
            }

            keys[i] = key;
            values[i] = value;
            N++;
        }
    }

    public void delete(Key key)
    {
     if (!contains(key)) return;
     int i = hash(key);
     //search the position
     while (!key.equals(keys[i]))
        i = (i + 1) % M;
     //search hit, replace it with null
     keys[i] = null;
     vals[i] = null;

     //re-put all the key that list after the target key
     i = (i + 1) % M;
     while (keys[i] != null)
     {
        Key   keyToRedo = keys[i];
        Value valToRedo = vals[i];
        keys[i] = null;
        vals[i] = null;
        N--;
        put(keyToRedo, valToRedo);
        i = (i + 1) % M;
     }
     N--;
     if (N > 0 N == M/8) resize(M/2);
     }
}
```
