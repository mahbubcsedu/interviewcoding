The prefix sum algorithm is a technique commonly used to preprocess an array to answer range-based queries efficiently. In a prefix sum array, each element at index \(i\) stores the sum of all elements from the start of the array up to \(i\). This allows us to quickly compute the sum of any subarray \([i, j]\) in constant time.

Here’s a breakdown of the prefix sum concept and problems from LeetCode and HackerRank that apply it.

### How Prefix Sum Works

1. **Create a prefix sum array**: Given an array `nums` of size `n`, create a prefix sum array `prefix` where each element `prefix[i]` is the sum of `nums[0]` to `nums[i]`.
   \[
   prefix[i] = nums[0] + nums[1] + ... + nums[i]
   \]

2. **Range Sum Query**: To find the sum of any subarray `[i, j]`, you can calculate it as:
   \[
   \text{subarray sum} = prefix[j] - prefix[i-1]
   \]
   if \(i > 0\), or simply `prefix[j]` if \(i = 0\).

This technique reduces the complexity of calculating subarray sums from \(O(n)\) to \(O(1)\) after a single \(O(n)\) preprocessing step.

---

### Problems Using Prefix Sum

Here's a list of problems with explanations and where they use the prefix sum technique:

#### 1. [Range Sum Query - Immutable (LeetCode #303)](https://leetcode.com/problems/range-sum-query-immutable/)
   - **Problem**: Given an integer array `nums`, calculate the sum of elements between indices `i` and `j` inclusive.
   - **Solution**: Construct a prefix sum array during initialization. Each range sum query can then be answered in \(O(1)\) using the prefix sum array.

#### 2. [Subarray Sum Equals K (LeetCode #560)](https://leetcode.com/problems/subarray-sum-equals-k/)
   - **Problem**: Find the total number of continuous subarrays that sum up to a given integer `k`.
   - **Solution**: Use prefix sums along with a hash map to store counts of prefix sums seen so far. For each prefix sum encountered, check if `prefix - k` exists in the map, which indicates a valid subarray sum.

#### 3. [Maximum Size Subarray Sum Equals k (LeetCode #325)](https://leetcode.com/problems/maximum-size-subarray-sum-equals-k/)
   - **Problem**: Find the maximum length of a subarray that sums to `k`.
   - **Solution**: Similar to the previous problem, but instead of counting subarrays, keep track of the maximum length found.

#### 4. [Find Pivot Index (LeetCode #724)](https://leetcode.com/problems/find-pivot-index/)
   - **Problem**: Find the "pivot" index where the sum of elements on the left is equal to the sum on the right.
   - **Solution**: Compute the total sum of the array, then iterate through the array while keeping a running sum of elements to the left. The condition becomes `2 * left_sum + nums[i] == total_sum`.

#### 5. [Product of Array Except Self (LeetCode #238)](https://leetcode.com/problems/product-of-array-except-self/)
   - **Problem**: Construct an array where each element at index `i` is the product of all elements in the array except `nums[i]`.
   - **Solution**: This is a variation that can be tackled using a prefix product array. Although not a sum, it’s a similar prefix technique where left products and right products are precomputed.

#### 6. [Minimum Size Subarray Sum (LeetCode #209)](https://leetcode.com/problems/minimum-size-subarray-sum/)
   - **Problem**: Find the minimal length of a contiguous subarray of which the sum is at least `s`.
   - **Solution**: While this problem can also be solved with the sliding window technique, prefix sums can be used to calculate sums efficiently.

#### 7. [Continuous Subarray Sum (LeetCode #523)](https://leetcode.com/problems/continuous-subarray-sum/)
   - **Problem**: Find if the array has a continuous subarray of size at least 2 that sums up to a multiple of `k`.
   - **Solution**: Use a prefix sum array and store the modulo values of the prefix sums in a hash map. If you encounter the same modulo again, it means there’s a subarray with a sum that is a multiple of `k`.

---

### Practice Strategy

1. **Start with Basic Range Queries**: Begin with problems like **Range Sum Query - Immutable** to get comfortable with creating and using prefix sums.
2. **Move to Hash Map-based Problems**: Try **Subarray Sum Equals K** and **Maximum Size Subarray Sum Equals k**, which combine prefix sums with hash maps.
3. **Advance to More Complex Problems**: Once you're comfortable, tackle **Continuous Subarray Sum** and **Minimum Size Subarray Sum**, which involve additional conditions or require a blend of techniques.

Using prefix sums can make what would otherwise be inefficient computations very quick.

Here are some more challenging problems involving prefix sums. These will deepen your understanding and application of the prefix sum technique, often requiring additional data structures or a combination of algorithms.

### Advanced Prefix Sum Problems

#### 1. [Count of Range Sum (LeetCode #327)](https://leetcode.com/problems/count-of-range-sum/)
   - **Problem**: Given an integer array `nums` and two integers `lower` and `upper`, return the number of range sums that lie in `[lower, upper]` inclusive.
   - **Solution**: This problem is a harder variation of the subarray sum problem. After computing prefix sums, you can use a **Segment Tree** or **Binary Indexed Tree (BIT)** to efficiently count ranges, or even a **merge sort approach** to count ranges in \(O(n \log n)\) time.

#### 2. [Maximum Sum of 3 Non-Overlapping Subarrays (LeetCode #689)](https://leetcode.com/problems/maximum-sum-of-3-non-overlapping-subarrays/)
   - **Problem**: Find three non-overlapping subarrays of length `k` with maximum sum.
   - **Solution**: Use prefix sums to precompute sums for each subarray of length `k`. Then, using dynamic programming and an additional pass over the prefix sums, calculate the maximum sum of three non-overlapping subarrays in \(O(n)\) time.

#### 3. [Range Sum Query 2D - Immutable (LeetCode #304)](https://leetcode.com/problems/range-sum-query-2d-immutable/)
   - **Problem**: Implement a 2D version of the range sum query, where you are given a 2D matrix and must answer sum queries for any rectangular submatrix.
   - **Solution**: This problem extends prefix sums to two dimensions. Build a 2D prefix sum array where each element at \((i, j)\) contains the sum of the matrix from the top-left corner \((0, 0)\) to \((i, j)\). Then you can compute the sum of any submatrix in constant time.

#### 4. [K-Sum Subarrays (LeetCode #1074)](https://leetcode.com/problems/number-of-submatrices-that-sum-to-target/)
   - **Problem**: Given a matrix and a target, find the number of submatrices that sum up to the target.
   - **Solution**: First, calculate prefix sums for the 2D array. Then, reduce each 2D submatrix to a 1D problem by fixing the top and bottom rows and applying a hash map-based prefix sum approach for each pair of rows, counting sums equal to the target.

#### 5. [Minimum Operations to Reduce X to Zero (LeetCode #1658)](https://leetcode.com/problems/minimum-operations-to-reduce-x-to-zero/)
   - **Problem**: You need to reduce `x` to zero by removing elements from the start or end of the array, minimizing the number of elements removed.
   - **Solution**: This problem can be transformed to finding the longest subarray with a sum equal to `total_sum - x`. Use prefix sums and a sliding window to find the longest subarray with that target sum, which leads to the minimum number of operations.

#### 6. [Maximum Sum of Two Non-Overlapping Subarrays (LeetCode #1031)](https://leetcode.com/problems/maximum-sum-of-two-non-overlapping-subarrays/)
   - **Problem**: Find the maximum sum of two non-overlapping subarrays of lengths `L` and `M`.
   - **Solution**: Compute prefix sums for each possible subarray of lengths `L` and `M`. Then, for each index, calculate the maximum possible sum by keeping track of the best choices for the subarrays to the left and right.

#### 7. [Split Array with Equal Sum (LeetCode #548)](https://leetcode.com/problems/split-array-with-equal-sum/)
   - **Problem**: Split the array into four parts with equal sums, and return `true` if possible.
   - **Solution**: Use prefix sums to find all possible points where a split can result in equal sums. Use nested loops and a hash set to optimize for each split position.

#### 8. [Binary Tree Maximum Path Sum (LeetCode #124)](https://leetcode.com/problems/binary-tree-maximum-path-sum/)
   - **Problem**: Given a binary tree, find the maximum path sum where the path may start and end at any node.
   - **Solution**: This isn’t a traditional prefix sum problem but involves computing cumulative sums for each path in a tree. You’ll need to use a recursive approach with backtracking to manage path sums and track the maximum possible path.

#### 9. [Stone Game VII (LeetCode #1690)](https://leetcode.com/problems/stone-game-vii/)
   - **Problem**: Two players are picking stones to maximize the difference between scores, with a given scoring rule based on removing stones from the start or end of an array.
   - **Solution**: Use dynamic programming with prefix sums to precompute the score for each possible subarray, then maximize the difference between players by calculating optimal scores.

---

### Tips for Tackling Harder Problems with Prefix Sum

1. **Combine Prefix Sums with Other Data Structures**: Many harder problems require combining prefix sums with hash maps, segment trees, binary indexed trees, or dynamic programming.
2. **2D Prefix Sums**: Extend your prefix sum understanding to two dimensions for matrix-related problems.
3. **Prefix Sum with Modulo**: For problems where you need to find specific remainder conditions, prefix sums modulo a given integer (e.g., modulo \(k\)) can help track the conditions.
4. **Optimize Space Complexity**: For very large arrays or matrices, consider in-place modifications of the input or using fewer auxiliary structures if possible.

Practice these advanced problems and explore optimizations with prefix sums for a comprehensive understanding.

The "Find Pivot Index" problem on LeetCode (#724) asks you to find an index in a list where the sum of all elements to the left of that index is equal to the sum of all elements to the right of it.

### Problem Statement
Given an array of integers `nums`, return the *pivot index* of this array. The pivot index is the index where the sum of all the numbers strictly to the left is equal to the sum of all the numbers strictly to the right.

If no such index exists, return `-1`. If there are multiple pivot indexes, return the left-most pivot index.

**Example:**

Input:
```plaintext
nums = [1, 7, 3, 6, 5, 6]
```

Output:
```plaintext
3
```

Explanation:
- The sum of the numbers to the left of index `3` (`[1, 7, 3]`) is `11`.
- The sum of the numbers to the right of index `3` (`[5, 6]`) is also `11`.

### Solution
The idea is to calculate the total sum of the array and then use a single pass to find the pivot index by maintaining a running sum. 

1. Calculate the total sum of the array.
2. Iterate through the array, updating the running sum of elements to the left of the current index.
3. For each index, check if twice the running sum plus the current element is equal to the total sum.

Here's the Python code to solve this problem:

```python
def pivotIndex(nums):
    total_sum = sum(nums)
    left_sum = 0
    
    for i, num in enumerate(nums):
        if left_sum == (total_sum - left_sum - num):
            return i
        left_sum += num
    
    return -1
```


### Explanation of the Code
1. **`total_sum = sum(nums)`**: Calculate the total sum of the array elements.
2. **Loop through each element**:
   - Check if the current left sum equals the right sum by comparing `left_sum == (total_sum - left_sum - num)`.
   - If they match, the current index `i` is the pivot index.
3. **Update `left_sum`** by adding the current element `num`.

### Time Complexity
- **O(n)**: where `n` is the length of the array. We go through the array once to calculate the total sum and once more to find the pivot index.

### Space Complexity
- **O(1)**: No extra space is required apart from a few variables.

## 1074. Number of Submatrices That Sum to Target ##

This solution is efficient for large arrays since it requires only a single pass after the initial sum calculation.
This problem is an extension of the "subarray sum equals target" problem but in two dimensions. Given a matrix of integers and a target, we need to count the number of non-empty submatrices that sum to the target.

### Approach

The main idea is to treat each possible pair of rows as defining the top and bottom of a submatrix and then use a similar approach to the "subarray sum equals target" problem to count submatrices that sum to the target.

1. **Define row boundaries**: For each pair of rows (`r1` and `r2`), we consider a compressed 1D array representing the column sums from `r1` to `r2`.
2. **Apply prefix sum and hash map**: For each pair of rows, calculate the column sums between those rows and store the prefix sums in a hash map to find subarrays that sum to the target.

### Steps

1. **Iterate over pairs of rows**. For each pair of rows (`r1`, `r2`):
   - Create a 1D array, `column_sums`, where each element `column_sums[c]` represents the sum of elements in column `c` between rows `r1` and `r2`.
   
2. **Use hash map to count subarrays**:
   - Calculate the prefix sums for `column_sums` and use a hash map to count the number of subarrays in `column_sums` that sum to the target.
   - This part works similarly to finding subarrays with a given sum in a 1D array.

Here's the code implementing this solution:

```python
def numSubmatrixSumTarget(matrix, target):
    rows, cols = len(matrix), len(matrix[0])
    result = 0
    
    # Iterate over each possible pair of rows
    for r1 in range(rows):
        # This will store the cumulative sum between row r1 and r2 for each column
        column_sums = [0] * cols
        for r2 in range(r1, rows):
            # Update the column sums for the current row r2
            for c in range(cols):
                column_sums[c] += matrix[r2][c]
            
            # Now find subarrays in column_sums that sum to target
            # Using a dictionary to count prefix sums
            prefix_sum_count = {0: 1}
            current_sum = 0
            for sum_val in column_sums:
                current_sum += sum_val
                # Check if there is a subarray that sums to `target`
                if current_sum - target in prefix_sum_count:
                    result += prefix_sum_count[current_sum - target]
                # Update prefix sum count
                prefix_sum_count[current_sum] = prefix_sum_count.get(current_sum, 0) + 1
    
    return result
```

### Explanation of the Code

1. **Row Pair Loop**: We iterate over all possible pairs of rows (`r1`, `r2`).
2. **Column Sums Calculation**: For each pair of rows, calculate the sum of elements between these two rows for each column.
3. **Prefix Sum Hash Map**:
   - For the `column_sums` array, calculate prefix sums and use a hash map to find the number of subarrays that sum to `target`.
   - This part works by checking if `current_sum - target` has been seen in `prefix_sum_count`.
   - If it has, we add the count of occurrences to the result.

### Complexity Analysis

- **Time Complexity**: \(O(\text{rows}^2 \times \text{cols})\). We iterate over all pairs of rows, and for each pair, we process each column and use a hash map to find subarrays with the given sum.
- **Space Complexity**: \(O(\text{cols})\). We store only the `column_sums` array and `prefix_sum_count` dictionary.

This approach efficiently finds the number of submatrices that sum to the target by reducing the problem to 1D subarray sums for each pair of rows.
