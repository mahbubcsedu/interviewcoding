### Line Sweep Algorithm Overview

The **Line Sweep Algorithm** is a computational geometry technique used to solve problems related to intervals or regions. It is particularly effective when we need to process a collection of intervals or events in an efficient manner, typically in a sorted order. This technique is often applied in problems involving **intervals, events, or ranges** in 1D, 2D, or even higher dimensions.

The core idea is to "sweep" a vertical or horizontal line across the problem space, processing events in the order they occur, usually sorted by position or time. As the line moves, we update or maintain a set of active intervals or events, and solve the problem as the line passes through different stages.

### Key Steps of the Line Sweep Algorithm:
1. **Define Events:** The problem is first broken down into a series of events. For example, in a 2D space, these could be the starting and ending points of intervals (or rectangles).
  
2. **Sort the Events:** These events are sorted, typically by their position along the axis (for 1D problems) or a combination of positions and other properties (like time).

3. **Process Events in Order:** Once the events are sorted, the algorithm processes them in order, maintaining a set of active intervals or objects. Depending on the problem, each event might correspond to adding or removing an element from the active set.

4. **Maintain Active Set:** As the line sweeps across the space, it maintains a dynamic data structure (often a priority queue, hash set, or balanced tree) that helps in efficiently querying or updating the current state of active intervals.

5. **Answer Queries or Count Results:** The result is typically built incrementally by updating the state based on the events. For example, in computational geometry, we might need to count overlapping intervals, or in scheduling problems, we might need to count parallel tasks.

### Example of How Line Sweep Works:
For example, consider the problem of finding the number of overlapping intervals:

- **Input:** `[(1, 3), (2, 4), (3, 5)]` (each tuple represents an interval with start and end times).
- **Events:** Create two types of events: starting and ending points of intervals.
    - Event 1: `(1, "start")`
    - Event 2: `(3, "end")`
    - Event 3: `(2, "start")`
    - Event 4: `(4, "end")`
    - Event 5: `(3, "start")`
    - Event 6: `(5, "end")`
  
- **Sort Events:** Sort by position. If two events have the same position, process "end" events before "start" events to handle cases of exact overlaps.
    - Sorted Events: `[(1, "start"), (2, "start"), (3, "start"), (3, "end"), (4, "end"), (5, "end")]`

- **Process Events:** Start with an empty set of active intervals. For each event:
    - Start event: Add the interval to the active set.
    - End event: Remove the interval from the active set.
    - Track the size of the active set to determine the maximum overlap.

### Leetcode Problems Solvable Using Line Sweep

Here is a list of significant problems from LeetCode that can be solved using the Line Sweep Algorithm:

1. **[Leetcode 56 - Merge Intervals](https://leetcode.com/problems/merge-intervals/)**
   - **Problem:** Given a collection of intervals, merge all overlapping intervals.
   - **Solution:** Sort intervals by start time and sweep through, merging overlapping ones.
  
2. **[Leetcode 57 - Insert Interval](https://leetcode.com/problems/insert-interval/)**
   - **Problem:** Given a set of non-overlapping intervals, insert a new interval and merge if necessary.
   - **Solution:** Use a sweep to find the correct position for the new interval and merge overlaps.

3. **[Leetcode 986 - Interval List Intersections](https://leetcode.com/problems/interval-list-intersections/)**
   - **Problem:** Given two lists of intervals, find the intersection of the intervals.
   - **Solution:** Sweep through both lists of intervals and find common ranges.

4. **[Leetcode 252 - Meeting Rooms](https://leetcode.com/problems/meeting-rooms/)**
   - **Problem:** Determine if a person can attend all meetings, given the start and end times.
   - **Solution:** Sort intervals and sweep through, checking for overlaps.

5. **[Leetcode 253 - Meeting Rooms II](https://leetcode.com/problems/meeting-rooms-ii/)**
   - **Problem:** Given intervals, determine the minimum number of meeting rooms required.
   - **Solution:** Sort intervals and use a priority queue to maintain the current state of meeting rooms.

6. **[Leetcode 315 - Count of Smaller Numbers After Self](https://leetcode.com/problems/count-of-smaller-numbers-after-self/)**
   - **Problem:** Given a list of numbers, count the number of smaller elements to the right of each element.
   - **Solution:** Use a line sweep with a binary search tree or a similar data structure to count elements efficiently.

7. **[Leetcode 218 - The Skyline Problem](https://leetcode.com/problems/the-skyline-problem/)**
   - **Problem:** Given a collection of buildings, return the outline (skyline) formed by the buildings' edges.
   - **Solution:** Use line sweep to handle the building's edges (start and end) and track height changes.

8. **[Leetcode 452 - Minimum Number of Arrows to Burst Balloons](https://leetcode.com/problems/minimum-number-of-arrows-to-burst-balloons/)**
   - **Problem:** Find the minimum number of arrows required to burst all balloons, where each balloon is represented as an interval on a line.
   - **Solution:** Sort the intervals and sweep through, using a greedy approach to burst the maximum number of balloons with each arrow.

9. **[Leetcode 163 - Missing Ranges](https://leetcode.com/problems/missing-ranges/)**
   - **Problem:** Given a sorted list of integers, find the ranges of missing numbers between the integers.
   - **Solution:** Use a line sweep to track the gaps between numbers and form the missing ranges.

10. **[Leetcode 334 - Increasing Triplet Subsequence](https://leetcode.com/problems/increasing-triplet-subsequence/)**
    - **Problem:** Find if there is an increasing subsequence of length 3 in an array.
    - **Solution:** Use a sweep approach with two variables to track the smallest increasing sequence.

### Techniques and Data Structures for Line Sweep:
- **Sorting:** Sorting the intervals or events by their start or end position is a key part of the line sweep.
- **Priority Queue (Min-Heap):** Often used to efficiently maintain and query the active intervals (e.g., in meeting room problems).
- **Balanced Tree (AVL, Segment Tree):** For range queries and updates (e.g., counting smaller numbers).
- **Hash Sets or Maps:** For managing and querying dynamic sets of intervals or events.

### Conclusion:
The **Line Sweep Algorithm** is a versatile and efficient technique for solving problems involving intervals or events in 1D or 2D space. It is often used to reduce the time complexity of problems that would otherwise require brute-force methods. The above list from LeetCode provides a variety of problems that can be tackled using line sweep, and the algorithm is commonly paired with sorting and data structures like heaps and balanced trees to handle active intervals or events dynamically.


The **"Count of Smaller Numbers After Self"** problem (LeetCode 315) requires you to count, for each element in an array, how many numbers to the right are smaller than that element. This problem is a great candidate for an efficient solution using **Line Sweep** and **Binary Search**.

### Problem Overview:
Given an array `nums`, for each element `nums[i]`, you need to find how many numbers to the right of `nums[i]` are smaller than `nums[i]`.

**Example:**
Input: `nums = [5, 2, 6, 1]`
Output: `[2, 1, 1, 0]`

**Explanation:**
- For `5`, the numbers smaller than it to the right are `2` and `1`. So the count is `2`.
- For `2`, the only number smaller than it to the right is `1`. So the count is `1`.
- For `6`, the number smaller than it to the right is `1`. So the count is `1`.
- For `1`, there are no numbers to the right smaller than it. So the count is `0`.

### Naive Approach:
A naive solution involves iterating through each element and checking all the elements to its right. This results in a time complexity of \(O(n^2)\), which is inefficient for large arrays.

### Efficient Approach Using Line Sweep and Binary Search:
We can improve this problem using a **Binary Search Tree (BST)** or **Binary Indexed Tree (BIT)** (also called a **Fenwick Tree**). We can think of the line sweep as "sweeping" from right to left through the array while keeping track of the smaller elements seen so far.

#### Strategy:
- Start from the rightmost element.
- Maintain a dynamic list (or data structure) of elements already processed (i.e., elements to the right of the current element in the original array).
- For each element `nums[i]`, count how many elements in this list are smaller than `nums[i]`. This can be done efficiently using a **Binary Search**.

### Detailed Steps:
1. **Initialize a Sorted Data Structure:** We will use a **SortedList** (from the `sortedcontainers` library) to maintain the elements that have been processed so far. This data structure supports binary search operations, which are ideal for this problem.
  
2. **For each element** (starting from the rightmost):
   - Use **binary search** to find the number of elements in the sorted list that are smaller than the current element.
   - Add the current element to the sorted list.
  
3. **Store the count** of elements smaller than the current element in a result array.

### Solution Code (using SortedList):

```python
from sortedcontainers import SortedList

def countSmaller(nums):
    result = []
    sorted_list = SortedList()  # This will keep the elements we have seen so far (sorted)
    
    # Traverse the nums array from right to left
    for num in reversed(nums):
        # Find the position of num in the sorted list (the count of numbers smaller than num)
        result.append(sorted_list.bisect_left(num))
        # Insert num into the sorted list
        sorted_list.add(num)
    
    # The result array is in reverse order, so reverse it to get the final result
    return result[::-1]

# Example usage
nums = [5, 2, 6, 1]
print(countSmaller(nums))  # Output: [2, 1, 1, 0]
```

### Explanation:

1. **Initialization:**
   - `result`: This will store the count of smaller numbers for each element.
   - `sorted_list`: This is a `SortedList` that stores the elements we have seen so far, in sorted order.

2. **Reverse Iteration:**
   - We loop through the array from the last element to the first (`reversed(nums)`).
   - For each element `num`:
     - We use `bisect_left(num)` to find the index of the first element in `sorted_list` that is greater than or equal to `num`. The index returned by `bisect_left(num)` represents the count of elements smaller than `num` in the `sorted_list` (since the list is sorted).
     - We append this count to the `result` array.
     - We then insert `num` into the `sorted_list` using `add(num)`.

3. **Final Result:**
   - After processing all elements, the `result` list will be in reverse order, so we return the reversed list (`result[::-1]`).

### Time Complexity:
- **Inserting into the SortedList**: Each insertion into the `SortedList` takes \(O(\log n)\).
- **Binary Search (bisect_left)**: Finding the position of `num` using binary search also takes \(O(\log n)\).
- Since we process each element once, the overall time complexity is \(O(n \log n)\), where `n` is the length of the array.

### Space Complexity:
- The space complexity is \(O(n)\) because we store the result array and the `SortedList`, which both grow to size \(n\).

### Example Walkthrough:

For `nums = [5, 2, 6, 1]`:

1. **Start with the last element (1):**
   - `sorted_list = []`
   - `bisect_left(1) = 0` (no elements are smaller than 1)
   - Add `1` to `sorted_list`: `sorted_list = [1]`
   - `result = [0]`

2. **Move to 6:**
   - `sorted_list = [1]`
   - `bisect_left(6) = 1` (1 element smaller than 6, which is 1)
   - Add `6` to `sorted_list`: `sorted_list = [1, 6]`
   - `result = [0, 1]`

3. **Move to 2:**
   - `sorted_list = [1, 6]`
   - `bisect_left(2) = 1` (1 element smaller than 2, which is 1)
   - Add `2` to `sorted_list`: `sorted_list = [1, 2, 6]`
   - `result = [0, 1, 1]`

4. **Move to 5:**
   - `sorted_list = [1, 2, 6]`
   - `bisect_left(5) = 2` (2 elements smaller than 5, which are 1 and 2)
   - Add `5` to `sorted_list`: `sorted_list = [1, 2, 5, 6]`
   - `result = [0, 1, 1, 2]`

Finally, reverse the result to get `[2, 1, 1, 0]`, which is the correct output.

### Conclusion:
The **Line Sweep Algorithm** combined with **Binary Search** (through a `SortedList`) provides an efficient solution to the "Count of Smaller Numbers After Self" problem. This approach significantly reduces the time complexity from \(O(n^2)\) to \(O(n \log n)\), making it feasible to solve larger input sizes.
