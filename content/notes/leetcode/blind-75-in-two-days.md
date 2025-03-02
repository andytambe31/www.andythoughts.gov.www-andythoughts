+++
date = '2025-03-01T12:38:12-05:00'
draft = false
title = 'Blind 75 in Two Days'
+++

After nearly two weeks of trying to get back into the groove of studying DSA, I’m ready to take on the infamous Blind 75 challenge. That said, I want to make it clear that this task is quite ambitious and not suited for beginners. A strong foundation in OOP, control flow, data structures, and design patterns is essential before attempting it. Now that the disclaimer is out of the way, let’s dive into the challenge!

### [Longest Consecutive Sequence](https://leetcode.com/problems/longest-consecutive-sequence)

> Difficulty: Medium

Given an unsorted array of integers `nums`, return the length of the longest consective elements sequence.

You must write an algorithm that runs in **O(n)**  time.

**Example 1:**
> Input: nums = [100, 4, 200, 1, 3, 2]
> Output: 4
> Explanation: The longest consective elements sequence is `1,2,3,4`. Therefor its length is 4.

**Solution**

To tackle this problem, we utilize a `HashSet` to store all unique integers from the array.  

Once the `HashSet` is populated, we iterate through it. For each value, we check if `value - 1` is absent in the set. If it exists, it means a longer sequence starts earlier, so we skip the current value. However, if there is no preceding value, we initiate the longest streak from the current number and continue building from there.

```java
class Solution {
    public int longestConsecutive(int[] nums) {
        Set<Integer> lookup = new HashSet<>();
        for(int num: nums)
            lookup.add(num);

        int longestStreak = 0;

        for(int num: lookup){
            if(!lookup.contains(num - 1)){
                int current = num;
                int streak = 1;

                while(lookup.contains(current + 1)){
                    streak++;
                    current++;
                }

                longestStreak = Math.max(longestStreak, streak);
            }
        }

        return longestStreak;
    }
}
```

### [Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-consecutive-sequence)

> Difficulty: Medium

Given a string `s`, find the length of the longest substring without duplicate characters.

**Example 1:**
> Input: s = "abcabcbb"
> Output: 3
> Explanation: The answer is "abc", with the length of 3.

**Solution**

We utilize the Sliding Window technique along with a `HashSet`, starting with both pointers, `left` and `right`, initialized at 0. As we traverse the string, new characters are added to the set from the right, while any necessary removals are performed from the left.  

Before adding a new character, we first check if it already exists in the set. If it does, we increment the left pointer, removing characters until the duplicate is no longer present.  

Once the new character is added from the right, we update `maxLength` accordingly.

```java
class Solution {
    public int lengthOfLongestSubstring(String s){
        Set<Character> lookup = new HashSet();
        int left = 0, right = 0, maxLength = 0;

        while(right < s.length()){
            
            while(lookup.contains(s.charAt(right))){
                lookup.remove(s.charAt(left));
                left++;
            }

            lookup.add(s.charAt(right));
            maxLength = Math.max(maxLength, right - left + 1);
            right++;
        }

        return maxLength;
    }
}
```

The above two questions took me 1 hour 30 mins to solve!

### [Longest Palindromic Substring](https://leetcode.com/problems/longest-palindromic-substring/?envType=problem-list-v2&envId=oizxjoit)

> Difficulty: Medium

Give a string `s`, return the longest palindromic substring in s.

**Example 1:**
> Input: s = "babad"
> Output: "bab"
> Explanation: "aba" is also a valid answer.

**Solution**

The question requires us to find and return the longest palindromic substring. Therefore, any algorithm we design must determine the starting (left) and ending (right) indices of a valid palindrome. Let's denote these indices as `i` and `j`.

**Conditions for a String to be Palindromic**

- Every substring of length **1** is palindromic.  
- Every substring of length **2** is palindromic if `s[i] == s[j]` (i.e., the two characters at indices `i` and `j` are the same).  
- If we define the difference between `i` and `j` as `diff`, where `diff` can be `2, 3, 4,` and so on, then `j = i + diff`. A substring from `i` to `j` is palindromic if:
  - The substring between `i + 1` and `j - 1` is also palindromic.
  - `s[i] == s[j]` (i.e., the first and last characters match).

Based on these conditions, we can utilize a dynamic programming approach with a boolean matrix to track palindromic substrings.

```java
int n = s.length();
boolean[][] dp = new boolean[n][n]; 
```

**Identifying all single-character substrings as palindromic**

Every single-character substring should be marked as palindromic by setting the corresponding entry in the `dp` matrix to `true`. In this context, each `dp[i][j]` entry represents whether `s.substring(i, j + 1)` is a palindrome, meaning if `dp[i][j] = true`, then the substring from index `i` to `j` (inclusive) is palindromic.

```java
for(int i = 0; i < n; i++)
    dp[i][i] = true;
```

**Identifying all two-character substrings as palindromic**

A two-character substring should be marked as palindromic if the characters at indices `i` and `i + 1` are identical.

```java
for(int i = 0; i < n - 1; i++)
    if(s.charAt(i) == s.charAt(i + 1)){
        dp[i][i+1] = true;
        ans[0];
        ans[1] = i + 1;
    }
```

**Identifying all substrings larger than two-character as palindromic**

Before identifying larger palindromic substrings, we assume that a three-character palindrome contains a one-character palindrome within it, just as a five-character palindrome contains a three-character one. The same applies to even-length substrings (e.g., a two-character palindrome within a four-character one). This justifies initially marking all one- and two-character substrings.

Thus, for longer palindromic substrings, the difference between `i` and `j` should start at 2 (for a three-character substring) and increment progressively (3, 4, etc.) until it reaches the maximum length of `s`.

```java
for(int diff = 2; diff < s.length; diff++)
    for(int i = 0; i < n - diff; i++){
        int j = i + diff;
        if(s.charAt(i) == s.charAt(j) && dp[i + 1][j -1]) {
            dp[i][j] = true;
            ans[0] = i;
            ans[1] = j;
        }
    }
```

Lastly, the indices for the longest palindromic substring is stored in `ans` array. So lets return the final substring,

```java
return s.substring(ans[0], ans[1] + 1); // substring in java [i, j), doesn't include j
```

**Source code**

```java
class Solution {
    public String longestPalindrome(String s) {
        int n = s.length();
        boolean[][] dp = new boolean[n][n];
        int[] ans = {0, 0};

        // Mark all 1 length substring as palindrome
        for(int i = 0; i < n; i++)
            dp[i][i] = true;


        // Mark 2 length subtring, if i & i + 1 are same
        for(int i = 0; i < n - 1; i++)
            if(s.charAt(i) == s.charAt(i + 1)){
                dp[i][i + 1] = true;
                ans[0] = i;
                ans[1] = i + 1;
            }

        // Beginning for difference between i & j as 3, 4, 5 ... max length of string
        for(int diff = 2; diff < n; diff++){
            for(int i = 0; i < n - diff; i++){
                int j = i + diff;
                if(s.charAt(i) == s.charAt(j) && dp[i + 1][j - 1]){
                    dp[i][j] = true;
                    ans[0] = i;
                    ans[1] = j;
                }
            }
        }

        int i = ans[0];
        int j = ans[1];
        return s.substring(i, j + 1);
    }
}
```

### [Graph Valid Tree](https://leetcode.com/problems/graph-valid-tree/description/?envType=problem-list-v2&envId=oizxjoit)

> Difficulty: Medium

You have a graph of n nodes labeled from 0 to n - 1. You are given an integer n and a list of edges where edges[i] = [ai, bi] indicates that there is an undirected edge between nodes ai and bi in the graph.

Return true if the edges of the given graph make up a valid tree, and false otherwise.

**Solution**

This question is essentially asking us to confirm that the graph is a single connected component and contains no cycles. Both validations can be performed using `dfs()` from the 0th node.

**Auxilary Data Structure**

- Build an adjacency list array `adj` to model the edges between nodes
- boolean array `visited` to track all visited nodes

```java
List<Integer>[] adj = new ArrayList[n]; // n - number of nodes
boolean[] visited = new boolean[n];
```

> Just constructing the adjancency list isn't enough, we must initialize for each index within the array.

```java
for(int i = 0; i < n; i++)
    adj[i] = new ArryayList<Integer>();
```

**Mapping edges**

Each edge in the list connects two nodes, represented by `edge[0]` and `edge[1]`.

```java
for(int[] edge: edges){
    adj[edge[0]].add(edge[1]);
    adj[edge[1]].add(edge[0]);
}
```

**Starting DFS**

We choose source node for dfs traversal as `0`. Since, this is going to be recursive function, we must pass the references to the following,

- Adjacency List (adj)
- Boolean Tracker array (visited)
- parent node (initially set to -1)
- current node in traversal ( initially set 0)

```java
dfs(adj, visited, -1, 0)
```

**Depth First Search**

This DFS implementation checks for cycles by detecting back edges. In a graph with a cycle, back edges exist, allowing traversal to a previously visited node through a different unvisited edge. This characteristic is what defines a back edge.

```java
private void dfs(List<Integer>[] adj,boolean[] visited, int parent, int v){
    visited[v] = true;
    for(int w: adj[v]){
        if(!visited[w]){
            dfs(adj, visited, v, w);
        }
        else if( w != parent){
            this.valid = false;
            return;
        }
    }
}
```

**Detecting Multiple components**

Once `dfs(0)` completes, all nodes connected to it will be marked as visited, leaving any unvisited nodes from separate components. Therefore, we simply need to check if any node remains unvisited.

```java
for (boolean v : visited) 
    if (!v) return false;
```

Finally, if all checks are successfully met, we can confirm that the graph is valid.

**Source Code**

```java
class Solution {
    private boolean valid = true;

    public boolean validTree(int n, int[][] edges) {
        if(edges.length != n - 1) return false;

        List<Integer>[] adj = new ArrayList[n];
        boolean[] visited = new boolean[n];

        for(int i = 0; i < adj.length; i++)
            adj[i] = new ArrayList<Integer>();
        
        for(int[] edge: edges){
            adj[edge[0]].add(edge[1]);
            adj[edge[1]].add(edge[0]);
        }

        dfs(adj, visited, -1, 0);

        // Check if there any components present in the graph
        for (boolean v : visited) {
            if (!v) return false;
        }

        return this.valid;
    }

    private void dfs(List<Integer>[] adj,boolean[] visited, int parent, int v){
        visited[v] = true;
        for(int w: adj[v]){
            if(!visited[w]){
                dfs(adj, visited, v, w);
            }
            else if( w != parent){
                this.valid = false;
                return;
            }
        }
    }
}
```

### [Clone Graph](https://leetcode.com/problems/clone-graph/?envType=problem-list-v2&envId=oizxjoit)

> Difficulty: Medium

Given a reference of a node in a connected undirected graph. Return a deep copy (clone) of the graph.

Each node in the graph contains a value (int) and a list (List[Node]) of its neighbors.

```java
class Node {
    public int val;
    public List<Node> neighbors;
}
```

**Solution**

This algorithm **clones an undirected graph** using **Depth-First Search (DFS)** and a **HashMap** to track visited nodes. It recursively creates a copy of each node and its neighbors while avoiding cycles. If a node has already been cloned, it returns the existing copy from the `visited` map. This ensures efficient cloning with a time complexity of **O(V + E)**. 🚀

```java
/*
// Definition for a Node.
class Node {
    public int val;
    public List<Node> neighbors;
    public Node() {
        val = 0;
        neighbors = new ArrayList<Node>();
    }
    public Node(int _val) {
        val = _val;
        neighbors = new ArrayList<Node>();
    }
    public Node(int _val, ArrayList<Node> _neighbors) {
        val = _val;
        neighbors = _neighbors;
    }
}
*/
class Solution {
    private Map<Node, Node> visited = new HashMap();

    public Node cloneGraph(Node node) {
        if(node == null) return node;

        if(visited.containsKey(node)) 
            return visited.get(node);

        Node v = new Node(node.val, new ArrayList<>());
        
        for(Node w: node.neighbors)
            v.neighbors.add(cloneGraph(w));

        return v;
    }
}
```

### [Remove Nth Node From End of List](https://leetcode.com/problems/remove-nth-node-from-end-of-list/description/?envType=problem-list-v2&envId=oizxjoit)

Given the head of a linked list, remove the nth node from the end of the list and return its head.

**Solution**

We could use a solution with two pointers, the first pointer advances the list by `n + 1` steps from the beginning, while the second pointer starts from the beginning of the list.

We now must maintain the constant gap by advancing both pointers together until the first pointer arrives past the last node. The second pointer will be pointing at the `nth` node counting from the last.

We relink the next pointer of the node reference by the second pointer to point to the node's next node.

**Shifting the first**

In order for the second pointer to be exactly one node before the node to be deleted, we must move first only n + 1 steps ahead. The reason behind this decision is that we can then simply perform, `second.next = second.next.next`. If we move first only `n` steps ahead, second will stop on the node we want to remove, making the deletion harder.

```java
for(int i = 0; i <= n; i++)  // this leads to an iteration of n + 1
    first = first.next;
```

**Traversing beyond last node**

After gap of `n+1` is achieved between first & second node, we must start the traversal of second & first towards the end. Be advised, we want first to go beyond the last node, so we'll only check if `first != null`.

```java
while(first != null){
    first = first.next;
    second = second.next;
}
```

Now, second is right before the node to removed. Thus we can simply perform, the `second.next = second.next.next`.

**Need for dummy node**

There is a scenario where the LinkedList contains only 1 node, and the value of n is also 1. In that case we want to create a dummy node to point to head, and start the iteration from it.

```java
ListNode dummy = new ListNode(0);
dummy.next = head;

ListNode first = dummy;
ListNode second = dummy;
```

**Source Code**

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        // Dummy node to handle edge cases
        ListNode dummy = new ListNode(0);
        dummy.next = head;

        ListNode first = dummy;
        ListNode second = dummy;

        // Move `first` pointer `n+1` steps ahead
        for (int i = 0; i <= n; i++) {
            first = first.next;
        }

        // Move `first` & `second` until `first` reaches the end
        while (first != null) {
            first = first.next;
            second = second.next;
        }

        // If we are removing the first node, return second.next directly
        if (second.next == head && n == 1) {
            return null;
        }

        // Remove the nth node
        second.next = second.next.next;

        return dummy.next; // Return updated head
    }
}
```
