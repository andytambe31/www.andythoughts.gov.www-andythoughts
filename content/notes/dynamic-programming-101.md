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
- Entries are filled **sequentially**, starting from the smallest subproblems up to the final solution