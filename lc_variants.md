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

