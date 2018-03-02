---
layout: post
title: "undirected-graph"
date: 2018-2-20 11:30:20
tag:
- data-structure
- graph
description: Graph Glossary, Graph API, Design Pattern for graph processing, Depth-first search, Breath-first search,Graph search and path API, Graph Connected Component Detect API.
comments: true
---
# Undirected Graph

<hr />

## Appendix

* Graph Glossary

* Graph API

* Search and Path
  * Depth-First-Search
  * Breath-first search

* Connected Component

* Bipartite graph

<hr />

## Graph Glossary

| Term           | Definition                                                  |
| :-------------: | :-------------:|
| Graph          | A graph is a set of vertices and a collection of edges that each connect a pair of vertices.|
| simple Graph   | A graph with no self-loop and no parallel edges             |
| Path           | Path in a graph is a sequence of vertices connected by edges, the length of Path is number of edges |
| Cycle          | A cycle is a path with at least one edge whose first and last vertices are the same     |
| Connected Graph|A graph is connected if there is a path from every vertex to every other vertex in the graph.|
| Acyclic Graph| An acyclic graph is a graph with no cycles|
| bipartite Graph       |a bipartite Graph is a graph whose vertices we can divide into two sets such that all edges connect a vertex in one set with a vertex in the other set.      |

<hr />

## Graph API

**There are three way to represent Graph**

* Adjacency list: best solution: seems like separating chaining in hashing.

* Adjacency matrix: sparse graph cause inefficient memory usage

* edge list: adj() will traversal through all edges, cost too much time.

### Basic Graph API

``` java
public class graph{
  private final int V;
  private int E;
  private Bag<Interger>[] adj;
  //constructor

  public graph(int V){
    this.V = V;
    this.E = 0;

    for(int i = 0; i < V; i++){
      adj[i] = new Bag<Interger>();
    }
  }

  //return number of vertices
  public int V() {
    return V;
  }

  // return number of edges
  public int E() {
    return E;
  }

  //add edges between v and w
  public void addedge(int v, int w){
    adj[v].add(w);
    adj[w].add(v);
    E++;
  }

  //return a Iterable object contain vertices that linked to v
  public Iterable<Integer> adj(int v){
    return adj[v];
  }

  //print graph
  public String toString() {
    String s = V + "vertices" + E + "edges\n";
    for(int v = 0; v<V; s++){
      s += v + ":";
      for(int w : adj(v)){
        s += w + "";
      }
      return s;
    }
  }
}

```

### Extended graph API

```java

/* The following function eliminates the main function, only show support function*/

public int degree(Graph G, int v){
  int degree;
  for(int w : G.adj(v)){
    degree ++;
  }
  return degree;
}

public int maxDegree(Graph G, int v){
  int max = 0;
  for(int v = 0; v < G.V(); v++){
    if(degree(G,v) > max){
      max = degree(G,v);
    }
  }
  return max;
}

public int hasLoops(Graph G){
  for(int v = 0; v < G.V(); v++){
    for (int w : G.adj(v)){
      if(v == w) return true;
    }
  }
  return false;
}
```

<hr />

## Search

**Find a path from v to w in a graph, record each vertices in the path.**

### DFS

**process the vertices until it has no vertex connected to**

```java
public class DFS {
  private boolean[] marked;
  private int[] edgeTo;
  private final int s;

  public DFS(Graph G, int s){
    marked = new boolean[G.V()];
    edgeTo = new int[G.V()];
    this.s = s;
    dfs(G,s);
  }

  private void dfs(Graph G, int s){
    marked[s] = true;
    for(int w : G.adj(s)){
      if(!marked[w]){
        edgeTo[v] = w;
        dfs(G,w);
      }
    }
  }

  public Iterable<Integer> pathTo(int v){
    Stack<Integer> path = new Stack<Integer>();
    for(int x = v; x!= s; x = edgeTo[x]){
      path.push(x);
    }
    path.push(s);
  }
}

```

### BFS

**Process all the branch first then go deep, this method always return the shortest path from source**

```java
public class BFS{
  private boolean[] marked;
  private int[] edgeTo;
  private final int s;

  public BreadthFirstPaths(Graph G, int s){
    marked = new boolean[G.V()];
    edgeTo = new int[G.V()];
    this.s = s;
    bfs(G, s);
  }

  public void bfs(Graph G, int s){
    Queue<Integer> queue = new Queue<Integer>();
    marked[s] = true;
    queue.enqueue(s);
    while(!queue.isempty()){
      int v = queue.dequeue();
      for (int w : G.adj(v)){
        if (!marked[w])       
        {
          edgeTo[w] = v;     
          marked[w] = true;  
          queue.enqueue(w);
        }
      }
    }
  }

  public Iterable<Integer> pathTo(int v){
    Stack<Integer> path = new Stack<Integer>();
    for(int x = v; x!= s; x = edgeTo[x]){
      path.push(x);
    }
    path.push(s);
  }

}
```

<hr />

## Connected Component.

**This class is used for detect how many components in a graph**

```java
public class CC{
  private boolean[] marked;
  private int[] id;
  private int count; //return how many component does the graph have

  public CC(Graph G){
    marked = new boolean[G.V()];
    id = new int [G. V()];
    //in order to record which components does each vertex belong to

    for(int s = 0; s < G.V(); s++){
      // if the graph is connected, if statement should be only execute once
      if(!marked[s]){
        dfs(G, s);
        count ++;
      }
    }
  }

  private void dfs(Graph G, int v){
    marked[v] = true;
    id[v] = count;
    for (int w : G.adj(v)){
      if (!marked[w]) {
        dfs(G, w);
      }
    }
  }

  public boolean connected(int v, int w)
  {  return id[v] == id[w];  }

  public int id(int v)
  {  return id[v];  }

  public int count()
  {  return count;  }
}
```

<hr />

## Bipartite Graph

**vertices who have same color can't be connected together**

```java
public class Bipartite{
  private boolean[] marked;
  private boolean[] color;
  private boolean isbipartite = true;

  public Bipartite(Graph G){
    marked = new boolean[G.V()];
    color = new boolean[G.V()];

    for(int s = 0; s < G.V(); s++){
      // in order to cover all components
      if(!marked[s]){
        dfs(G, s);
      }
    }
  }

  private void dfs(Graph G, int v){
    marked[v] = true;
    for (int w : G.adj(v)){
      if (!marked[w]) {
        color[w] = !color[v];
        dfs(G, w);
      }
      else if (color[w] == color[v]) isTwoColorable = false;
    }
  }

  public boolean isBipartite()
  {  return isTwoColorable;  }
}
```
<hr />
