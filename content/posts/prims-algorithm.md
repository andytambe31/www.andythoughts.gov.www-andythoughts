+++
date = '2025-02-28T14:19:05-05:00'
draft = true
title = 'Prims Algorithm'
+++

Prim's algorithm, categorized as "greedy algorithm", is used to compute any undirected weighted graph's MST (Minimum Spanning Tree). It uses a [Priority Queue](https://algs4.cs.princeton.edu/24pq/) as an auxilary data structure, to store the least expensive [Edge](https://algs4.cs.princeton.edu/code/javadoc/edu/princeton/cs/algs4/Edge.html) that connects any vertex to the MST, currently not part of the MST.

Each selected Edge can be added to Queue signifying the MST. The queue's size would be **V - 1**.

There are two implementations of Prim's algorithm
- Lazy implementation - leaves unselected edges in the Priority Queue
- Eager implementation - uses a [IndexMinPQ](https://algs4.cs.princeton.edu/43mst/IndexMinPQ.java.html) instead to perform decrease-key operation.