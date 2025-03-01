+++
date = '2025-02-28T14:19:05-05:00'
draft = true
title = 'Prims Algorithm'
+++

Prim's algorithm, categorized as "greedy algorithm", is used to compute any undirected weighted graph's MST (Minimum Spanning Tree). It uses a [Priority Queue](https://algs4.cs.princeton.edu/24pq/) as an auxilary data structure, to store the least expensive [Edge](https://algs4.cs.princeton.edu/code/javadoc/edu/princeton/cs/algs4/Edge.html) that connects any vertex to the MST, currently not part of the MST.

Each selected Edge can be added to Queue signifying the MST. The queue's size would be **V - 1**. The edge weights can be positive, zero, or negative and need not be distinct. If the graph is not connected, then it computes a minimum spanning forest. The `weight()` returns the total weight of a minimum spanning tree, and the `edges()` method returns its edges.

There are two implementations of Prim's algorithm
- Lazy implementation - leaves unselected edges in the Priority Queue
- Eager implementation - uses a [IndexMinPQ](https://algs4.cs.princeton.edu/43mst/IndexMinPQ.java.html) instead to perform decrease-key operation.

### Eager Prim's algorithm

This implementation of Prim's algorithm uses an indexed binary heap as the underlying data structure for the Indexed Minimum Priority Queue.

#### Algorithm breakdown

The algorithm builds an MST incrementally by always selecting the minimum weight edge that connects a new vertex to the existing MST.

**Data structures used**

`edgeTo[v]` - Stores the minimum-weight edge that connects `v` to MST
`distTo[v]` - Stores the minimum edge weight to connect `v` to the MST
`marked[v]` - boolean array that tracks whether a vertex is included in the MST
`pq` - Priority queue to keep track of the next minimum weight edge