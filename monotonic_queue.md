Monotonic queues are useful data structures that help solve problems requiring the minimum or maximum of a sliding window over an array in an efficient way. The queue maintains elements in a specific order (either increasing or decreasing) as you iterate through the array, making it efficient to access the min or max values in \(O(1)\) time.

Here are some LeetCode problems that are well-suited for practicing the monotonic queue concept.

---

### Problems Using Monotonic Queue

#### 1. [Sliding Window Maximum (LeetCode #239)](https://leetcode.com/problems/sliding-window-maximum/)
   - **Problem**: Given an array `nums` and a window size `k`, return an array of the maximum values in each sliding window.
   - **Solution**: Use a monotonic decreasing queue to store indices of `nums`, ensuring the largest element of each window is always at the front of the queue. This enables efficient max queries within each window.

#### 2. [Shortest Subarray with Sum at Least K (LeetCode #862)](https://leetcode.com/problems/shortest-subarray-with-sum-at-least-k/)
   - **Problem**: Given an integer array `nums` and an integer `k`, find the shortest non-empty subarray with a sum at least `k`.
   - **Solution**: Use a prefix sum array and a monotonic queue to maintain potential starting points of the shortest subarray. A monotonic queue helps by storing indices where the prefix sum values are increasing, allowing efficient range sum checks for the shortest subarray.

#### 3. [Minimum Window Subsequence (LeetCode #727)](https://leetcode.com/problems/minimum-window-subsequence/)
   - **Problem**: Given strings `S` and `T`, find the minimum window in `S` that contains `T` as a subsequence.
   - **Solution**: Although not a sliding window maximum/minimum problem directly, using a deque can help maintain potential subsequences efficiently, by keeping track of minimal windows while ensuring increasing order for certain comparisons.

#### 4. [Constrained Subsequence Sum (LeetCode #1425)](https://leetcode.com/problems/constrained-subsequence-sum/)
   - **Problem**: Given an integer array `nums` and an integer `k`, find the maximum sum of a subsequence such that for every two consecutive elements in the subsequence, the absolute difference in their indices is at most `k`.
   - **Solution**: Use a monotonic deque to maintain the maximum of the previous `k` values, allowing efficient calculation of the maximum sum within the constrained window.

#### 5. [Longest Continuous Subarray with Absolute Diff Less Than or Equal to Limit (LeetCode #1438)](https://leetcode.com/problems/longest-continuous-subarray-with-absolute-diff-less-than-or-equal-to-limit/)
   - **Problem**: Find the longest subarray in which the absolute difference between any two elements is less than or equal to `limit`.
   - **Solution**: Use two monotonic queues (one for the maximum values and one for the minimum values) to keep track of the current window. Adjust the window size dynamically based on whether the difference exceeds the limit, maintaining the invariant of the longest subarray that satisfies the condition.

#### 6. [Max Sliding Window Sum with a Twist (LeetCode #1696)](https://leetcode.com/problems/jump-game-vi/)
   - **Problem**: You are jumping across an array, with each jump adding or subtracting from your score based on array values. Find the maximum possible score you can achieve given that you can jump a maximum of `k` steps forward.
   - **Solution**: Use a monotonic queue to keep track of the best scores within each possible jump window. As you progress, the queue helps find the optimal path by efficiently retrieving the max score achievable within each jump constraint.

#### 7. [Minimum Number of Removals to Make Mountain Array (LeetCode #1671)](https://leetcode.com/problems/minimum-number-of-removals-to-make-mountain-array/)
   - **Problem**: Given an array, determine the minimum number of removals required to make the array a mountain (increasing to a peak, then decreasing).
   - **Solution**: This problem requires a bit of dynamic programming, but a monotonic queue can help find peaks and manage subarrays for each side of the peak efficiently.

---

### Tips for Using Monotonic Queues

1. **Keep Only Relevant Elements**: For both maximum and minimum queries, remove elements that fall out of the current window from the queue.
2. **Maintain Order**: Use a monotonic decreasing queue for maximums and a monotonic increasing queue for minimums.
3. **Consider Double Queues**: For problems with both max and min constraints, using two queues simultaneously can help maintain both conditions efficiently.

### Practice Strategy

1. **Start with Sliding Window Maximum**: This is the classic monotonic queue problem and will help you get comfortable with basic operations.
2. **Work Through Sum Constraints**: Problems like "Shortest Subarray with Sum at Least K" add complexity by involving range sums, making them a good next step.
3. **Tackle Constraint and Jump Problems**: "Constrained Subsequence Sum" and "Jump Game VI" require careful management of subsequences within constraints, solidifying your understanding of maintaining queues over varying ranges. 

With these problems, you'll gain a strong grasp of the monotonic queue technique and how it helps optimize sliding window and constraint-based scenarios.
