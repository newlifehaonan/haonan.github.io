---
layout: post
title: "directed graph"
date: 2018-2-25 11:30:20
tag:
- data-structure
- graph
description: Digraph API, Directed Acyclic graph API, DepthFirstOrder traversal API, topological sort, Strong connectivity component API.
comments: true
---
# Directed Graph

<hr />

## Appendix

* Digraph Glossary

* Digraph API

* Digraph traversal
  * Depth-First-Directed-Path
  * Breath-first-Directed-Path

* Depth first order

* DAGS ( Directed acyclic graph)

* Topological sort

* Strong connected graph

<hr />

## Digraph Glossary

| Term | Definition    |
| :-------------: | :-------------: |
| Directed Graph       | A directed graph (or digraph) is a set of vertices and a collection of directed edges. directed edge points from the first vertex in the pair and points to the second vertex in the pair.      |
| OutDegree     | The OutDegree of a vertex in a digraph is the number of edges going from it.     |
| InDegree | the InDegree of a vertex is the number of edges going to it. |
| directed path       |  a sequence of vertices in which there is a (directed) edge pointing from each vertex in the sequence to its successor in the sequence.       |
| directed cycle     | a directed path with at least one edge whose first and last vertices are the same.     |
|directed acyclic graph (DAG)|A directed acyclic graph (DAG) is a digraph with no directed cycles.|
|strongly connected|Two vertices v and w are strongly connected if they are mutually reachable.  A digraph is strongly connected if all its vertices are strongly connected to one another.|

<hr />

## Digraph
```java
public class Digraph{
  public final int V;
  public int E;
  private Bag<Integer>[] adj;

  public Digraph(int V) {
    this.V = V;
    this.E = 0;
    adj = new Bag<Integer>[V];
    for(int v = 0; v<V; v++){
      adj[v] = new Bag<Integer>();
    }
  }

  public int V()  {  return V;  }
  public int E()  {  return E;  }


  public void addEdge(int v, int w){
    adj[v].add(w);
    E++;
  }

  public Iterable<Integer> adj<int v>{
    return adj[v];
  }

  public Digraph reverse(){
    Digraph R = new Digraph(V);
    for(int v=0; v<V; v++){
      for(int w : adj(v)){
        R.addEdge(w,v);
      }
    }
    return R;
  }

```
<hr />

## Digraph traversal

### Directed DFS
```java
public class DepthFirstDirectedPaths {
	private final boolean[] marked;  // marked[v] = true if v is reachable from s
	private final int[] edgeTo;      // edgeTo[v] = last edge on path from s to v
	private final int s;       // source vertex

	// single source
	public DepthFirstDirectedPaths(Digraph G, int s) {
		marked = new boolean[G.V()];
		edgeTo = new int[G.V()];
		this.s = s;
		dfs(G, s);
	}


	private void dfs(Digraph G, int v) {
		marked[v] = true;
		for (int w : G.adj(v)) {
			if (!marked[w]) {
				edgeTo[w] = v;
				dfs(G, w);
			}
		}
	}

	// is there a directed path from s to v?
	public boolean hasPathTo(int v) {
		return marked[v];
	}

	// return path from s to v; null if no such path
	public Iterable<Integer> pathTo(int v) {
		if (!hasPathTo(v)) return null;
		Stack<Integer> path = new Stack<>();
		for (int x = v; x != s; x = edgeTo[x])
			path.push(x);
		path.push(s);
		return path;
	}
}
```

### Directed BFS

```java
public class BreadthFirstDirectedPaths {
	private static int INFINITY = Integer.MAX_VALUE;
	private boolean[] marked;  // marked[v] = is there an s->v path?
	private int[] edgeTo;      // edgeTo[v] = last edge on shortest s->v path
	private int[] distTo;      // distTo[v] = length of shortest s->v path

	// single source
	public BreadthFirstDirectedPaths(Digraph G, int s) {
		marked = new boolean[G.V()];
		distTo = new int[G.V()];
		edgeTo = new int[G.V()];
		for (int v = 0; v < G.V(); v++) distTo[v] = INFINITY;
		bfs(G, s);
	}

	// multiple sources
	public BreadthFirstDirectedPaths(Digraph G, Iterable<Integer> sources) {
		marked = new boolean[G.V()];
		distTo = new int[G.V()];
		edgeTo = new int[G.V()];
		for (int v = 0; v < G.V(); v++) distTo[v] = INFINITY;
		bfs(G, sources);
	}

	// BFS from single source
	private void bfs(Digraph G, int s) {
		Queue<Integer> q = new Queue<>();
		marked[s] = true;
		distTo[s] = 0;
		q.enqueue(s);
		while (!q.isEmpty()) {
			int v = q.dequeue();
			for (int w : G.adj(v)) {
				if (!marked[w]) {
					edgeTo[w] = v;
					distTo[w] = distTo[v] + 1;
					marked[w] = true; // for BFS, mark as you enqueue
					q.enqueue(w);
				}
			}
		}
	}

	// BFS from multiple sources
	private void bfs(Digraph G, Iterable<Integer> sources) {
		Queue<Integer> q = new Queue<>();
		for (int s : sources) {
			marked[s] = true;
			distTo[s] = 0;
			q.enqueue(s);
		}
		while (!q.isEmpty()) {
			int v = q.dequeue();
			for (int w : G.adj(v)) {
				if (!marked[w]) {
					edgeTo[w] = v;
					distTo[w] = distTo[v] + 1;
					marked[w] = true;
					q.enqueue(w);
				}
			}
		}
	}

	// length of shortest path from s (or sources) to v
	public int distTo(int v) {
		return distTo[v];
	}

	// is there a directed path from s (or sources) to v?
	public boolean hasPathTo(int v) {
		return marked[v];
	}

	// shortest path from s (or sources) to v; null if no such path
	public Iterable<Integer> pathTo(int v) {
		if (!hasPathTo(v)) return null;
		Stack<Integer> path = new Stack<>();
		int x;
		for (x = v; distTo[x] != 0; x = edgeTo[x])
			path.push(x);
		path.push(x);
		return path;
	}
}
```
<hr />

## DAGS(Directed acyclic graph)

**Detect whether the graph is a directed acyclic graph, do it has a cycle? if true, put the cycle path into a stack so that we can trace the cycle.**

```java
public class DAG {
  private int marked[];
  private int edgeTo[];
  private Stack<Integer> cycle;
  private int onStack[];

  public DAG(Digraph G) {
    onStack = new boolean[G.V()];
    edgeTo  = new int[G.V()];
    marked  = new boolean[G.V()];
    for (int v = 0; v < G.V(); v++){
      if (!marked[v]) dfs(G, v);
    }
  }
  private void dfs(Graph G, int v) {
    marked[v] = true;
    onStack[v] = true;
    for(int w : G.adj(v)){
      if(this.hasCycle()) return;
      else if(!marked[w]){
        edgeTo[w] =v;
        dfs(G,w);
      }
      else if(onStack[w]) {
        cycle = new Stack<Integer>();
        for (int x =v; x!= w; x = edgeTo[x]){
          cycle.push(x);
        }
        cycle.push(w);
        cycle.push(v);
      }
      onStack[v] = false;
    }
  }

  public boolean hasCycle()
  {  return cycle != null;  }

  public Iterable<Integer> cycle()
  {  return cycle;  }

}
```
<hr />

## Depth First Order

**There are three vertex ordering：**

* Preorder : Put the vertex on a queue before the recursive calls.
* Postorder : Put the vertex on a queue after the recursive calls.
* Reverse postorder : Put the vertex on a stack after the recursive calls.

```java
public class DepthFirstOrder
  {
     private boolean[] marked;
     private Queue<Integer> pre;
     private Queue<Integer> post;
     private Stack<Integer> reversePost;  // vertices in reverse postorder
     public DepthFirstOrder(Digraph G)
     {
        pre           = new Queue<Integer>();
        post          = new Queue<Integer>();
        reversePost   = new Stack<Integer>();
        marked  = new boolean[G.V()];
        for (int v = 0; v < G.V(); v++)
            if (!marked[v]) dfs(G, v);
}
     private void dfs(Digraph G, int v)
     {
        pre.enqueue(v);
        marked[v] = true;
        for (int w : G.adj(v))
           if (!marked[w])
              dfs(G, w);
        post.enqueue(v);
        reversePost.push(v);
}
     public Iterable<Integer> pre()
     {  return pre;  }
     public Iterable<Integer> post()
     {  return post;  }
     public Iterable<Integer> reversePost()
     {  return reversePost;  }
}

```

<hr />

## Topological sort

**Precedence-constrained scheduling amounts to computing a topological order for the vertices of a DAG**

**reversePost order of a DAG is exactly the order of Topological sort**
* step 1: is the graph acyclic?

* step 2: what is it's reversepost order?

```java
public class Topological
{
  private Iterable<Integer> order;

  public Topological(Digraph G){
    DirectedCycle cyclefinder = new DirectedCycle(G);
    if (!cyclefinder.hasCycle())
    {
      DepthFirstOrder dfs = new DepthFirstOrder(G);
      order = dfs.reversePost();
    }
  }

  public Iterable<Integer> order()
  {  return order;  }

  public boolean isDAG()
  {  return order == null; }
}
```

<hr />

## Strong connected graph.
**Compute CC on a reversePost order of a reverse graph of the original graph**

* step 1 : compute Topological sort on a reverse graph of original graph

* step2 : Do DFS following the Topological sort order on original graph.

```java
public class KosarajuSCC
 {
private boolean[] marked;
private int[] id;
private int count;
// reached vertices
// component identifiers
// number of strong components
public KosarajuSCC(Digraph G)
{
   marked = new boolean[G.V()];
   id = new int[G.V()];
   DepthFirstOrder order = new DepthFirstOrder(G.reverse());
   for (int s : order.reversePost())
      if (!marked[s])
      {  dfs(G, s); count++;  }
}
     private void dfs(Digraph G, int v)
     {
        marked[v] = true;
        id[v] = count;
        for (int w : G.adj(v))
           if (!marked[w])
               dfs(G, w);
}
     public boolean stronglyConnected(int v, int w)
     {  return id[v] == id[w];  }
     public int id(int v)
     {  return id[v];  }
     public int count()
     {  return count;  }
}

```
<hr />
