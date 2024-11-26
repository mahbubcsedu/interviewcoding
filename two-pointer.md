**A good two pointer**
**3097: Shortest Subarray With OR at Least K II**
You are given an array nums of non-negative integers and an integer k.

An array is called special if the bitwise OR of all of its elements is at least k.

Return the length of the shortest special non-empty 
subarray
 of nums, or return -1 if no special subarray exists.

 
```python
Example 1:

Input: nums = [1,2,3], k = 2

Output: 1

Explanation:

The subarray [3] has OR value of 3. Hence, we return 1.

Example 2:

Input: nums = [2,1,8], k = 10

Output: 3

Explanation:

The subarray [2,1,8] has OR value of 11. Hence, we return 3.

Example 3:

Input: nums = [1,2], k = 0

Output: 1

Explanation:

The subarray [1] has OR value of 1. Hence, we return 1.
```

```python
class Solution:
    def minimumSubarrayLength(self, nums: List[int], k: int) -> int:
        prefix_sum=[]
        # 10 or 01 = 11 = 3
        # 11 or 10 = 11 = 3

        # main observation is number cnnot exceed 32 bit, so have to ask about it

        # cumulative bit count
        def update_bit_count(bit_count, num, delta):
            for pos in range(32):
                if num & (1 << pos):
                    bit_count[pos] += delta
        
            
        # cumulative sum
        def convert_bits(bit_counts):
            converted_num = 0
            for pos in range(32):
                if bit_counts[pos]:
                    converted_num |= 1 << pos
            return converted_num
        
        min_len = float('inf')
        start, end = 0, 0
        bit_counts = [0] * 32

        while end < len(nums):
            update_bit_count(bit_counts, nums[end], 1)
            while start <= end and convert_bits(bit_counts) >= k:
                min_len=min(min_len, end-start+1)
                update_bit_count(bit_counts, nums[start], -1)
                start +=1
            end+=1
        
        return -1 if min_len == float('inf') else min_len
```
