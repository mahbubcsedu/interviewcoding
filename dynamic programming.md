Dynamic Programming (DP) is a technique for solving problems by breaking them down into simpler subproblems and solving each subproblem just once, storing the results to avoid redundant calculations. DP is often used for optimization problems where decisions build upon previous ones.

To truly **fathom dynamic programming** and prepare for **medium to expert-level interviews**, you should focus on mastering different **DP patterns**. Below is a **comprehensive list of problems** categorized by patterns and difficulty. These problems will cover a wide array of common DP techniques and applications.

---

## **1. Classical DP Patterns**

### **1.1. Fibonacci-Type Problems**

These problems involve computing a sequence, where each value depends on previous ones.

- **Climbing Stairs**
  - Problem: You are climbing a staircase. Each time you can either climb 1 or 2 steps. How many distinct ways can you reach the top?
  - [Leetcode: Climbing Stairs](https://leetcode.com/problems/climbing-stairs/)

- **House Robber**
  - Problem: You are a robber and want to rob houses along a street, but cannot rob two adjacent houses. Find the maximum amount of money you can rob.
  - [Leetcode: House Robber](https://leetcode.com/problems/house-robber/)

- **Fibonacci Number**
  - Problem: Find the nth Fibonacci number.
  - [Leetcode: Fibonacci Number](https://leetcode.com/problems/fibonacci-number/)

- **Stairs Problem (Generalized Fibonacci)**
  - Problem: You are climbing a staircase. At each step, you can climb 1, 2, or 3 steps. Find the number of ways to reach the top.
  - [Leetcode: Stairs Problem](https://leetcode.com/problems/number-of-ways-to-climb-stairs/)

---

### **1.2. Subset/Combination Problems**

These problems require counting the number of subsets or combinations that satisfy certain conditions.

- **Subset Sum Problem**
  - Problem: Given a set of numbers, check if there exists a subset whose sum is equal to a target.
  - [Leetcode: Partition Equal Subset Sum](https://leetcode.com/problems/partition-equal-subset-sum/)

- **Target Sum**
  - Problem: Given a set of numbers, find the number of ways to assign + or - signs to make the sum equal to a target value.
  - [Leetcode: Target Sum](https://leetcode.com/problems/target-sum/)

- **Combination Sum IV**
  - Problem: Given an array of integers and a target sum, find the number of ways to reach the target sum by summing up elements from the array.
  - [Leetcode: Combination Sum IV](https://leetcode.com/problems/combination-sum-iv/)

- **Count All Possible Palindromic Substrings**
  - Problem: Given a string, count how many distinct palindromic substrings it has.
  - [Leetcode: Palindromic Substrings](https://leetcode.com/problems/palindromic-substrings/)

---

### **1.3. Knapsack-Style Problems**

These problems are about making decisions where you choose an item or exclude it based on some weight or capacity constraints.

- **0/1 Knapsack**
  - Problem: You are given a set of items with weights and values. You have a knapsack with a maximum weight limit. Maximize the total value of items that you can carry.
  - [Leetcode: Knapsack Problem](https://leetcode.com/problems/partition-equal-subset-sum/)

- **Unbounded Knapsack**
  - Problem: Similar to the 0/1 knapsack, but you can choose items multiple times.
  - [Leetcode: Unbounded Knapsack](https://leetcode.com/problems/coin-change/)

- **Rod Cutting Problem**
  - Problem: Given a rod of length `n` and prices for different lengths, find the maximum profit you can make by cutting the rod into smaller lengths.
  - [Leetcode: Rod Cutting](https://leetcode.com/problems/rod-cutting/)

- **Coin Change Problem**
  - Problem: Given coins of different denominations and a total amount, find the minimum number of coins needed to make up that amount.
  - [Leetcode: Coin Change](https://leetcode.com/problems/coin-change/)

---

### **1.4. Longest Common Subsequence (LCS) Problems**

These problems are related to comparing two sequences and finding the longest subsequence that can be derived from both.

- **Longest Common Subsequence**
  - Problem: Given two sequences, find the length of their longest common subsequence.
  - [Leetcode: Longest Common Subsequence](https://leetcode.com/problems/longest-common-subsequence/)

- **Edit Distance**
  - Problem: Given two strings, find the minimum number of operations (insert, delete, or replace) required to convert one string into the other.
  - [Leetcode: Edit Distance](https://leetcode.com/problems/edit-distance/)

- **Longest Palindromic Subsequence**
  - Problem: Find the longest subsequence of a string that is a palindrome.
  - [Leetcode: Longest Palindromic Subsequence](https://leetcode.com/problems/longest-palindromic-subsequence/)

---

## **2. Advanced DP Patterns**

### **2.1. DP on Strings**

These problems focus on solving string-specific challenges using DP.

- **Word Break**
  - Problem: Given a string and a dictionary of words, determine if the string can be segmented into space-separated words from the dictionary.
  - [Leetcode: Word Break](https://leetcode.com/problems/word-break/)

- **Minimum Window Substring**
  - Problem: Given two strings `s` and `t`, find the smallest substring in `s` that contains all characters of `t`.
  - [Leetcode: Minimum Window Substring](https://leetcode.com/problems/minimum-window-substring/)

- **Regular Expression Matching**
  - Problem: Implement regular expression matching with support for `.` and `*`.
  - [Leetcode: Regular Expression Matching](https://leetcode.com/problems/regular-expression-matching/)

- **Longest Palindromic Substring**
  - Problem: Given a string, return the longest palindromic substring.
  - [Leetcode: Longest Palindromic Substring](https://leetcode.com/problems/longest-palindromic-substring/)

---

### **2.2. DP on Sequences**

These problems deal with finding optimal sequences based on constraints.

- **Longest Increasing Subsequence (LIS)**
  - Problem: Find the longest strictly increasing subsequence in a sequence of numbers.
  - [Leetcode: Longest Increasing Subsequence](https://leetcode.com/problems/longest-increasing-subsequence/)

- **Minimum Path Sum**
  - Problem: Given a grid, find the minimum path sum from the top-left corner to the bottom-right corner, only moving down or right.
  - [Leetcode: Minimum Path Sum](https://leetcode.com/problems/minimum-path-sum/)

- **Unique Paths**
  - Problem: Find the number of unique paths from the top-left corner to the bottom-right corner of a grid.
  - [Leetcode: Unique Paths](https://leetcode.com/problems/unique-paths/)

- **Maximum Product Subarray**
  - Problem: Find the contiguous subarray within an array that has the largest product.
  - [Leetcode: Maximum Product Subarray](https://leetcode.com/problems/maximum-product-subarray/)

---

### **2.3. DP on Trees/Graphs**

These problems involve using DP to optimize traversals and path calculations on trees or graphs.

- **Binary Tree Maximum Path Sum**
  - Problem: Given a binary tree, find the maximum path sum where the path can start and end at any node.
  - [Leetcode: Binary Tree Maximum Path Sum](https://leetcode.com/problems/binary-tree-maximum-path-sum/)

- **Word Break II**
  - Problem: Given a string and a dictionary, find all possible sentence segmentations of the string.
  - [Leetcode: Word Break II](https://leetcode.com/problems/word-break-ii/)

- **Longest Increasing Path in a Matrix**
  - Problem: Given an `m x n` matrix of integers, find the length of the longest increasing path.
  - [Leetcode: Longest Increasing Path in a Matrix](https://leetcode.com/problems/longest-increasing-path-in-a-matrix/)

---

### **2.4. DP with Bitmasking**

These problems are about efficiently solving problems with the help of bitmasking.

- **Traveling Salesman Problem (TSP)**
  - Problem: Given a set of cities and the distances between them, find the shortest possible route that visits each city exactly once and returns to the origin.
  - [Leetcode: Traveling Salesman Problem](https://leetcode.com/problems/traveling-salesman-problem/)

- **Subset Sum Problem (Using Bitmasking)**
  - Problem: Given a set, find all possible subsets using bitmasking.
  - [Leetcode: Subset Sum Problem](https://leetcode.com/problems/partition-equal-subset-sum/)

---

### **3. Miscellaneous Hard DP Problems**

These are advanced DP problems that combine multiple concepts and require deeper understanding of the DP technique.

- **Best Time to Buy and Sell Stock IV**
  - Problem: You are given a series of stock prices. Find the maximum profit you can achieve with at most `k` transactions.
  - [Leetcode: Best Time to Buy and Sell Stock IV](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iv/)

- **
