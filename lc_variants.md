## Neeted list weight sum 339

To modify the solution to multiply each integer by its reverse depth (i.e., depth from bottom up), we need to first calculate the maximum depth of the nested list. Once we have this information, we can use a similar depth-first search (DFS) approach to calculate the weighted sum based on the reverse depth.

Here's how you can implement this in Python:

1. **Calculate the Maximum Depth**: Use a DFS to find the maximum depth of the nested list.
2. **Calculate the Weighted Sum**: Use another DFS to calculate the sum, multiplying each integer by its reverse depth.

Here's the complete implementation:

```python
def depthSumInverse(nestedList):
    def get_max_depth(nested_list, depth):
        max_depth = depth
        for element in nested_list:
            if isinstance(element, list):
                max_depth = max(max_depth, get_max_depth(element, depth + 1))
        return max_depth

    def dfs(nested_list, depth, max_depth):
        total = 0
        for element in nested_list:
            if isinstance(element, int):
                total += element * (max_depth - depth + 1)
            else:
                total += dfs(element, depth + 1, max_depth)
        return total

    max_depth = get_max_depth(nestedList, 1)
    return dfs(nestedList, 1, max_depth)

# Example usage:
nestedList = [1, [2, 2], [[3], 2], 1]
print(depthSumInverse(nestedList))  # Output: 20
```

### Explanation:

1. **Get Maximum Depth**:
   - The `get_max_depth` function traverses the nested list and returns the maximum depth of the list.
   
2. **Depth-First Search (DFS)**:
   - The `dfs` function calculates the weighted sum based on the reverse depth. The reverse depth is calculated as `(max_depth - depth + 1)`.
   - For each element, if it is an integer, it multiplies the integer by its reverse depth and adds it to the total sum.
   - If the element is a list, it recursively processes the list with an increased depth.

### Example:
For the nested list `[1, [2, 2], [[3], 2], 1]`:
- Maximum depth is 3.
- Reverse depths:
  - `1` at depth 1 -> reverse depth 3
  - `2` at depth 2 -> reverse depth 2
  - `3` at depth 3 -> reverse depth 1
- The total sum is calculated as: `1*3 + 2*2 + 2*2 + 3*1 + 2*2 + 1*3 = 3 + 4 + 4 + 3 + 4 + 3 = 21`.

## First and last position in an array - variant
You can efficiently find the number of elements within an inclusive range in a sorted list using binary search. The idea is to use binary search to find the index of the first element that is greater than or equal to the lower bound of the range, and the index of the first element that is greater than the upper bound of the range. The difference between these indices will give the number of elements in the range. Here's a Python function to do this:

```python
import bisect

def count_elements_in_range(sorted_list, lower_bound, upper_bound):
    # Find the index of the first element greater than or equal to lower_bound
    left_index = bisect.bisect_left(sorted_list, lower_bound)
    
    # Find the index of the first element greater than upper_bound
    right_index = bisect.bisect_right(sorted_list, upper_bound)
    
    # The number of elements in the range is the difference between right_index and left_index
    count = right_index - left_index
    
    return count

# Example usage:
sorted_list = [1, 3, 3, 5, 7, 8, 10, 12]
lower_bound = 3
upper_bound = 8
print(count_elements_in_range(sorted_list, lower_bound, upper_bound))  # Output: 5
```

## Stock buy and sale variant
- Given two arrays of departure prices and return prices find the minimum total cost of a trip (return has to be on the same day or after departure). Eg: D = [10, 7, 8 , 8, 9] and R = [8, 10, 7, 8, 9] -> 14 (depart on day 2 and return on day 3).

You can solve this problem by iterating through both the departure and return arrays, and finding the minimum total cost of a trip where the return happens on the same day or after the departure. Here is a Python function that accomplishes this:

```python
def min_total_cost(departure_prices, return_prices):
    n = len(departure_prices)
    min_cost = float('inf')
    
    for i in range(n):
        for j in range(i, n):
            total_cost = departure_prices[i] + return_prices[j]
            min_cost = min(min_cost, total_cost)
    
    return min_cost

# Example usage:
D = [10, 7, 8, 8, 9]
R = [8, 10, 7, 8, 9]
print(min_total_cost(D, R))  # Output: 14
```

In this function, we iterate through each departure day `i` and for each `i`, we iterate through each return day `j` starting from `i`. We calculate the total cost for each combination and keep track of the minimum total cost found.
## Find kth largest elements when k is 1-based
```python
import random
class Solution:
    

    def partition(self, nums, left, right):
        pivot_index = random.randint(left, right)
        pivot_value = nums[pivot_index]
        nums[pivot_index], nums[right] = nums[right], nums[pivot_index]
        store_index = left
        
        for i in range(left, right):
            if nums[i] > pivot_value:
                nums[store_index], nums[i] = nums[i], nums[store_index]
                store_index += 1
        
        nums[right], nums[store_index] = nums[store_index], nums[right]
        return store_index

    def quickselect(self, nums, left, right, k):
        if left == right:
            return nums[left]
        
        pivot_index = self.partition(nums, left, right)
        
        if k == pivot_index:
            return nums[k]
        elif k < pivot_index:
            return self.quickselect(nums, left, pivot_index - 1, k)
        else:
            return self.quickselect(nums, pivot_index + 1, right, k)

    def findKthLargest(self, nums, k):
        return self.quickselect(nums, 0, len(nums) - 1, k-1)

# Example usage:
nums = [3, 2, 1, 5, 6, 4]
k = 2  # Find the 3rd largest element (zero-based index)
print(Solution().findKthLargest(nums, k))  # Output: 4
```

## Find kth largest elements when k is zero based
Sure! The Quickselect algorithm is an efficient way to find the k-th largest element in an unsorted array. Quickselect works similarly to Quicksort, but it only partially sorts the array to find the desired element.

Here's the Python implementation to find the k-th largest element using Quickselect, where k is zero-based (i.e., k=0 finds the largest element, k=1 finds the second largest element, and so on):

```python
import random

def partition(nums, left, right):
    pivot_index = random.randint(left, right)
    pivot_value = nums[pivot_index]
    nums[pivot_index], nums[right] = nums[right], nums[pivot_index]
    store_index = left
    
    for i in range(left, right):
        if nums[i] > pivot_value:
            nums[store_index], nums[i] = nums[i], nums[store_index]
            store_index += 1
    
    nums[right], nums[store_index] = nums[store_index], nums[right]
    return store_index

def quickselect(nums, left, right, k):
    if left == right:
        return nums[left]
    
    pivot_index = partition(nums, left, right)
    
    if k == pivot_index:
        return nums[k]
    elif k < pivot_index:
        return quickselect(nums, left, pivot_index - 1, k)
    else:
        return quickselect(nums, pivot_index + 1, right, k)

def findKthLargest(nums, k):
    return quickselect(nums, 0, len(nums) - 1, k)

# Example usage:
nums = [3, 2, 1, 5, 6, 4]
k = 2  # Find the 3rd largest element (zero-based index)
print(findKthLargest(nums, k))  # Output: 4
```

### Explanation:
1. **Partition Function**: The `partition` function selects a random pivot element and partitions the array such that all elements greater than the pivot are on the left, and all elements less than or equal to the pivot are on the right.
2. **Quickselect Function**: The `quickselect` function uses the `partition` function to recursively narrow down the search space to find the k-th largest element.
   - If the pivot index is equal to k, we have found the k-th largest element.
   - If k is less than the pivot index, we continue the search in the left subarray.
   - If k is greater than the pivot index, we continue the search in the right subarray.
3. **Find K-th Largest Function**: The `findKthLargest` function initializes the `quickselect` function with the entire array.

This implementation ensures an efficient way to find the k-th largest element with an average time complexity of \(O(n)\).

## 560 subarray sum equal k
-When dealing with the problem of finding the number of subarrays that sum to a given value \(k\), if the numbers provided are positive only, we can make significant improvements to the algorithm. Specifically, we can use a sliding window technique to achieve a more efficient solution.

### Sliding Window Approach

In this case, since all numbers are positive, the sum of the subarray will only increase as we add more elements to it. This allows us to use a sliding window approach to maintain a valid subarray whose sum is equal to \(k\).

Here’s the Python implementation:

```python
def subarraySum(nums, k):
    start, end = 0, 0
    current_sum = 0
    count = 0

    while end < len(nums):
        # Add the current element to the current_sum
        current_sum += nums[end]

        # Move the start pointer to the right while the current_sum is greater than k
        while current_sum > k and start <= end:
            current_sum -= nums[start]
            start += 1

        # If current_sum equals k, increment the count
        if current_sum == k:
            count += 1

        # Move the end pointer to the right
        end += 1

    return count

# Example usage:
nums = [1, 2, 3, 4, 5]
k = 5
print(subarraySum(nums, k))  # Output: 2 (subarrays are [2, 3] and [5])
```

### Explanation:
1. **Initialization**: Initialize `start` and `end` pointers, `current_sum` to keep track of the sum of elements in the window, and `count` to keep track of the number of subarrays that sum to \(k\).
2. **Expanding the Window**: Incrementally add elements to `current_sum` using the `end` pointer.
3. **Shrinking the Window**: If `current_sum` exceeds \(k\), increment the `start` pointer and subtract elements from `current_sum` until `current_sum` is less than or equal to \(k\).
4. **Counting Subarrays**: If `current_sum` equals \(k\), increment the `count`.
5. **Moving the End Pointer**: Continue expanding the window by moving the `end` pointer to the right.
