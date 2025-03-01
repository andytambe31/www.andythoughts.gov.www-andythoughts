+++
date = '2025-02-28T14:43:47-05:00'
draft = false
title = 'Indexed Minimum Priority Queue'
+++

Indexed Minimum Priority Queue is a data structure that manages a set of values, which will be referred to as `keys`, each associated with a unique index between 0 and maxN - 1, to be specified by the client.

It allows inserting keys, removing the smallest key, updating a key, and deleting a key. The index lets the user refer to a specific key when making changes to the priority queue.

Indexed Priorty Queue:

```java
indexMinPQ.insert(3, "apple"); // Inserts the key "apple" with index 3, where 3 is used to reference it later.
```

Priority Queue:

```java
pq.add("apple"); // Inserts "apple" based on its priority.
```

## Public APIs

In order to create an Indexed Priority Queue, the client needs to specify the `maxN` capacity.

```java
public IndexMinPQ(int maxN) {
    if( maxN < 0 ) thow new IllegalArgumentException();
    this.maxN = max.N;
    n = 0; // total number of keys
    keys = (Key[]) new Comparable[maxN]; // keys[i] = priority of i
    pq = new int[maxN]; // binary heap 
    qp = new int[maxN]; 
    for(int i = 0; i <= maxN; i++)
        qp[i] = -1;
}
```

A key difference between a standard Priority Queue and `IndexMinPQ` is the **insert operation**, which updates multiple internal arrays.  

The `pq` array represents a [**binary heap**](https://en.wikipedia.org/wiki/Binary_heap#/media/File:Max-Heap.svg), storing key indices `i`, while `qp` provides an inverse mapping for quick lookups. If `qp[i] = l`, index `i` is at position `l` in `pq`.  

In the snippet below, key `k` is stored in `keys[i]`, `i` is added to `pq` in heap order after `swim()` operation is performed, and `qp` tracks `i`’s position within `pq`.

```java
public void insert(int i, Key key){
    if(contains(i)) throw new IllegalArgumentException("index is already in the priority queue");
    n++;
    qp[i] = n;
    pq[n] = i;
    keys[i] = key;
    swim(n);
}
```

For deleting the minimum key and returning its index `i`, we can use the `delMin()` operation. The `delMin()` operation removes and returns the index of the **minimum key** by swapping it with the last element, reducing the heap size, and restoring heap order using `sink()`. It then updates `qp` and `keys` to mark the element as deleted, ensuring O(log n) efficiency.

```java
public int delMin() {
    if (n == 0) throw new NoSuchElementException("Priority queue underflow");
    
    int min = pq[0];    // Store the index of the minimum key
    exch(0, n - 1);     // Swap root with the last element
    n--;                // Reduce the size of the heap
    sink(0);            // Restore heap order

    qp[min] = -1;       // Mark index as deleted
    pq[n] = -1;         // Clear stale reference (was pq[n+1])
    keys[min] = null;   // Help garbage collection

    return min;         // Return the removed index
}
```

For changing the key associated with item `i`, we can use the `change(i, newKey)` operation.

```java
public void changeKey(int i, Key key){
    if (!contains(i)) throw new NoSuchElementException("index is not in the priority queue");
    keys[i] = key;
    swim(qp[i]);
    sink(qp[i]);
}
```

To decrease the key associated with index `i`, we use `decreaseKey(i, newKey)`. The key difference between `changeKey()` and `decreaseKey()` is that in `decreaseKey()`, the `newKey` must be strictly smaller than the current key. As a result, the key moves up in the heap to maintain the min-heap property, making it optimized to perform only the `swim()` operation.

```java
public void decreaseKey(int i, Key newKey){
    if(!contains(i)) throw new IllegalArgumentException("index is not in the priority queue");

    if(keys[i].compareTo(key) == 0)
        throw new IllegalArgumentException("Calling decreaseKey() with a key equal to the key in the priority queue");
    else if(keys[i].compareTo(key) < 0 )
        throw new IllegalArgumentException("Calling decreaseKey() with a key equal to the key in the priority queue");

    keys[i] = key;
    swim(qp[i]);
}
```

Additional APIs of the Indexed Minimum Priority Queue are,

isEmpty()

```java
public boolean isEmpty(){
    return n == 0;
}
```

contains(int i)

If key `k` was never inserted at index `i`, `qp[i]` will retain its initial value of `-1`. Thus, if `qp[i]` equals `-1`, it indicates that index `i` was never added.
```java
public boolean contains(int i){
    return qp[i] != -1;
}
```

size()

```java
public int size(){
    return n;
}
```

minIndex()

```java
public int minIndex(){
    return pq[0];
}
```

minKey()

```java
public Key minKey(){
    return keys[minIndex()];
}
```
