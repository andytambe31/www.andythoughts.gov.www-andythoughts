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
public void insert(int i, Item item){
    if(contains(i)) throw new IllegalArgumentException("index is already in the priority queue");
    qp[i] = n;
    pq[n] = i;
    items[i] = item;
    n++;
    swim(n - 1); // As the pq stores values from 0 to N - 1
}
```

For deleting the minimum key and returning its index `i`, we can use the `delMin()` operation. The `delMin()` operation removes and returns the index of the **minimum key** by swapping it with the last element, reducing the heap size, and restoring heap order using `sink()`. It then updates `qp` and `keys` to mark the element as deleted, ensuring O(log n) efficiency.

```java
public int delMin() {
    if (n == 0) throw new NoSuchElementException("Priority queue underflow");

    int min = pq[0];
    exch(0, n - 1);
    n--;
    sink(0);
    qp[min] = -1;
    pq[n] = -1;
    items[min] = null;
    return min;
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

### Keys to be stored

If we take the following sample input, where `Key`s are `String` values:

```java
String[] strings = { "it", "was", "the", "best", "of", "times", "it", "was", "the", "worst" };
```

The expected priority order for the given sequence is:

```md
"best" < "it" < "it" < "of" < "the" < "the" < "times" < "was" < "was" < "worst"
```

The resulting priority queue summary is:

```
Priority Queue Items
0 -> it	1 -> was	2 -> the	3 -> best	4 -> of	5 -> times	6 -> it	7 -> was	8 -> the	9 -> worst	

Priority Queue pq
0 -> 3	1 -> 0	2 -> 4	3 -> 6	4 -> 8	5 -> 5	6 -> 2	7 -> 7	8 -> 1	9 -> 9	
```

### Source Code

```java
package dev.aniruddhatambe.queues;

public class IndexMinPQ<Item extends Comparable<Item>> {
    private int n;
    private int[] pq;
    private int[] qp;
    private Item[] items;

    public IndexMinPQ(int maxN) {
        if(maxN <= 0) throw new IllegalArgumentException();
        n = 0;
        pq = new int[maxN];
        qp = new int[maxN];
        items = (Item[]) new Comparable[maxN];
        for(int i = 0; i < maxN; i++)
            qp[i] = -1;
    }

    public void insert(int i, Item item){
        if(contains(i)) throw new IllegalArgumentException("index is already in the priority queue");
        qp[i] = n;
        pq[n] = i;
        items[i] = item;
        n++;
        swim(n - 1);
    }

    public int delMin(){
        int min = pq[0];
        exch(0, n - 1);
        n--;
        sink(0);
        qp[min] = -1;
        pq[n] = -1;
        items[min] = null;
        return min;
    }

    public void changeKey(int i, Item item){
        if(!contains(i)) throw new IllegalArgumentException("index is not in the priority queue");
        items[i] = item;
        sink(qp[i]);
        swim(qp[i]);
    }

    public void decreaseKey(int i, Item item){
        if(!contains(i)) throw new IllegalArgumentException("index is not in the priority queue");
        if(items[i].compareTo(item) <= 0) throw new IllegalArgumentException("Calling decreaseKey() with a key equal or greater to the key in the priority queue");
        items[i] = item;
        sink(qp[i]);
    }

    public void increaseKey(int i, Item item){
        if(!contains(i)) throw new IllegalArgumentException("index is not in the priority queue");
        if (items[i].compareTo(item) >= 0) throw new IllegalArgumentException("Calling increaseKey() with a key equal or smaller to the key in the priority queue");
        items[i] = item;
        sink(qp[i]);
    }

    private void sink(int k){
        while(2 * k + 1 < n){
            int j = 2 * k + 1;
            if(j + 1 < n && greater(j, j+1)) j++;
            if(!greater(k, j)) break;
            exch(k, j);
            k = j;
        }
    }

    public boolean contains(int i){
        return qp[i] != -1;
    }

    private void swim(int k){
        while(k > 0 && k < n && greater(k/2, k)){
            exch(k,k/2);
            k = k/2;
        }
    }

    // All internal helper functions receive pq indices
    private boolean greater(int i, int j){
        return items[pq[i]].compareTo(items[pq[j]]) > 0;
    }

    private void exch(int i, int j){
        int swap = pq[i];
        pq[i] = pq[j];
        pq[j] = swap;
        qp[pq[i]] = i; // qp[pq[i]] = pq[qp[i]] = i must be true
        qp[pq[j]] = j;
    }

    public void summary(){
        System.out.println("Priority Queue Items");
        for(int i = 0; i < n; i++)
            System.out.print(i + " -> " + items[i] + "\t" );

        System.out.println("\n\nPriority Queue pq");
        for(int i = 0; i < n; i++)
            System.out.print(i + " -> " + pq[i]  + "\t");

        System.out.println("\n\nPriority Queue qp");
        for(int i = 0; i < n; i++)
            System.out.print(i + " -> " + qp[i]  + "\t");

    }
}
```