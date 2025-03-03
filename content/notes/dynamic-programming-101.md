+++
date = '2025-03-03T10:38:09-05:00'
draft = false
title = 'Dynamic Programming 101'
+++

Dynamic Programming (DP) problems can be generally classified into six main types, depending on how subproblems are organized and how states evolve:

1. **Linear DP**  
2. **Interval DP**  
3. **Two-Dimensional/Grid DP**  
4. **Knapsack DP**  
5. **Tree DP / Graph DP**  
6. **Bitmask DP**  

However, these categories can be further distilled into two fundamental approaches:  

1. **Top-Down (Memoization)**  
2. **Bottom-Up (Tabulation)**

**Differences in Memoization & Tabulation**
Both techniques are commonly used to tackle problems with overlapping subproblems (where the same subproblem is computed multiple times).

### **Memoization:**  

- **Top-down approach**  
- Stores the results of **function calls** in a table  
- Implemented using **recursion**  
- Entries are populated **on demand** when needed  

### **Tabulation:**  

- **Bottom-up approach**  
- Stores the results of **subproblems** in a table  
- Implemented using **iteration**  
- Entries are filled **sequentially**, starting from the smallest subproblems up to the final solution.

## What is state?
In **Dynamic Programming (DP)**, a **state** represents a distinct subproblem that contributes to solving the overall problem. A DP state generally consists of:

- **Parameters** that define the subproblem (e.g., index, capacity, or remaining elements).  
- **A value** representing the optimal solution for that subproblem.  
- **A transition relation** that determines how one state leads to another.  

In DP, we establish a **state transition equation**, which expresses the problem's solution in terms of smaller subproblems.

For example, in the **Fibonacci sequence**, the state equation is:

```java
dp[i] = dp[i - 1] + dp[i - 2]
```

Here, the **transition relation** defines how we progress from one state to the next—`dp[i]` is derived by summing `dp[i-1]` and `dp[i-2]`. Each computed value is then stored in `dp[i]` to avoid redundant calculations. Additionally, the variable that helps us transition to other states, could be defined as state variable. In this case, its `i`.

### **Key Takeaway**  
A **state** in DP serves as a structured representation of a subproblem, helping to build the final solution efficiently.