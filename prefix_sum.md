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
