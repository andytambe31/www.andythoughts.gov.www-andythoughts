+++
date = '2025-03-03T12:57:08-05:00'
draft = true
title = 'Longest Increasing Subsequence'
+++

Given an integer array nums, return the length of the longest strictly increasing subsequence.

**Example 1**

Input: nums = [10,9,2,5,3,7,101,18]

Output: 4

Explanation: The longest increasing subsequence is [2,3,7,101], therefore the length is 4.

### Solution

There are 3 ways you can solve this problem,

1. Dynamic Programming using bottom-up approach
2. Build optimum sub-sequence
3. Improve build of sub-sequence with binary search

## Dynamic Programming

A **bottom-up** dynamic programming approach is well-suited for solving this problem. However, we must first identify which are the subproblems. We can begin by assumming,

- Each number in the array is an increasing sub-sequence of length 1
- If the previous number (`i - 1`) is smaller than the current number (`i`), the subsequence length at `i` can be **length at `i - 1` plus 1**

With our basic assumptions in place, we can define the state container, `dp`, which will be an `int[]` array of size `nums.length`. Alongwith that, we must codify `Each number in the array is an increasing sub-sequence of length 1` by settin each entry in `dp` as 1;

```java
int[] dp = new int[nums.length];
Arrays.fill(dp, 1);
```

Now, let's implement the second assumption. As we traverse the array, the current number at `i` can extend an increasing subsequence if it is greater than any number between `0` and `i-1`. In such cases, we update `dp[i]` by taking the maximum length found so far and adding `1` to the subsequence ending at `j`.  

```java
for (int i = 0; i < nums.length; i++) 
    for (int j = 0; j < i; j++) 
        if (nums[i] > nums[j]) 
            dp[i] = Math.max(dp[i], dp[j] + 1);
```

At this point, we have calculated the length of each increasing subsequence. The final step is to find the maximum value in `dp`.

```java
int max = 1;
for(int length: num)
    max = Math.max(length, max)
```

That's it!

**Source Code**
```java
class Solution {
    public int lengthOfLIS(int[] nums) {
        int[] dp = new int[nums.length];
        Arrays.fill(dp, 1);

        for(int i = 0; i < nums.length; i++)
            for(int j = 0; j < i; j++)
                if( nums[i] > nums[j])
                    dp[i] = Math.max(dp[i], dp[j] + 1);
        
        int max = 0;
        for(int length: dp)
            max = Math.max(length, max);
        
        return max;
    }
}
```

The time complexity of this algorithm is **O(n²)**.

## Build optimum sub-sequence

The focus of this approach is to build an increasing subsequence, denoted as sub, which remains partially sorted due to its increasing nature. As we traverse the array, we check if the current number at i is greater than the last element of sub. If it is, we append it to sub, extending the subsequence. Otherwise, we find the correct position using linearSearch(sub, num) and replace the existing element to maintain the smallest possible values at each position while preserving the subsequence length.

```java
for(int num: nums)
    if(num > sub.get(sub.size() - 1))
        sub.add(num);
    else {
        int id = linearSearch(sub, num);
        sub.set(id, num);
    }
```

Linear Search

```java
private int linearSearch(List<Integer> sub, int key){
    int i = 0;
    while(key > sub.get(i))
        i++;
    return i;
}
```

After the traversal is complete, we must return the length of the subsequence.

```java
return sub.size();
```

**Source Code**
```java
class Solution {
    public int lengthOfLIS(int[] nums) {
        List<Integer> sub = new ArrayList<>();
        sub.add(nums[0]);

        for (int num : nums) {
            if (num > sub.get(sub.size() - 1)) {
                sub.add(num);
            } else {
                int j = linearSearch(sub, num);
                sub.set(j, num);
            }
        }
        return sub.size();
    }

    private int linearSearch(List<Integer> sub, int key){
        int i = 0;
        while(key > sub.get(i))
            i++;
        return i;
    }
}
```

**Time Complexity: O(n²)**

## Binary Search

In the above implementation, a key part of the algorithm involves finding the correct position to insert the current number in the subsequence, which was done using a linear search. Since the subsequence is partially sorted, we can optimize this by using binary search, reducing the time complexity from `O(n²)` to `O(n log n)`.

```java
class Solution {
    public int lengthOfLIS(int[] nums) {
        List<Integer> sub = new ArrayList<>();
        sub.add(nums[0]);

        for (int num : nums) {
            if (num > sub.get(sub.size() - 1)) {
                sub.add(num);
            } else {
                int j = binarySearch(sub, num);
                sub.set(j, num);
            }
        }
        return sub.size();
    }

    private int binarySearch(List<Integer> sub, int key){
        int lo = 0;
        int hi = sub.size() - 1;

        while(lo < hi){
            int mid = lo + (hi - lo) / 2;

            if(sub.get(mid) == key)
                return mid;

            if(sub.get(mid) < key)
                lo = mid + 1;
            
            else
                hi = mid;
        }

        return lo;
    }
}
```