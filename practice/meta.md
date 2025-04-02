'''python
class Node():
    def __init__(self, val=0, left=None, right=None):
        self.value = val 
        self.left = left
        self.right = right


class Solution:
    
    def __init__(self): 
        self.diameter=0

    def dfs(self, node):
        if not node:
            return 0
        lh = self.dfs(node.left) 
        rh = self.dfs(node.right)
        self.diameter = max(self.diameter, lh+rh)
        return 1+max(rh, lh)

    def get_diameter(self, root: Optional[Node]):

        self.dfs(root)
        return self.diameter

"""
          1
          /\
        2   6
        /\  / 
        3 4 7 
          / /
          5 8
"""

t = Node(1, Node(2, Node(3), Node(4, Node(5))), Node(6, Node(7, Node(8))))
d=Solution().get_diameter(t)
print(d)
```

```python


from typing import Optional


import random 

class Solution:

    def partition(self, arr, left, right):
        # choose a random pivot index
        # partition around that where left contains largest and right contains smallest
        # return pivot index after partition 

        p_index = random.randint(left, right)
        p_val = arr[p_index]

        arr[p_index], arr[right] = arr[right], arr[p_index]
        store_index = left 

        for start in range(left, right):
            if arr[start] > p_val:
                arr[start], arr[store_index] = arr[store_index], arr[start]
                store_index+=1
        
        arr[store_index], arr[right] = arr[right], arr[store_index]
        return store_index

    def quickselect(self, low, high, arr, k):
        if low==high:
            return arr[low]
        pivot_index = self.partition(arr, low, high)

        if k==pivot_index:
            return arr[k]
        elif k < pivot_index:
            return self.quickselect(low, pivot_index-1, arr, k)
        else:
            return self.quickselect(pivot_index+1, high, arr, k)


    def kth_largest(self, arr, k):
        # check for validation 
        # call quck select and return value
        # k is 1 based, array is zero based 
        if k > len(arr):
            raise ValueError("Not possible")
        
        return self.quickselect(0, len(arr)-1, arr, k-1)
        # pass  


o = Solution()
print(o.kth_largest([3,2,1,5,6,9], 3))

```

```python
# 23 + 34 * 3/2+1
# last=23, cur = 34, pre_op=+, cur_character is operator when we are making decision 
import math
class Solution:

    def basic_calculator(self, s:str):
        # s+="+"
        
        # not readable code

        eval_val = 0
        last_num = 0
        cur_num = 0
        previous_operator = "+"

        for index, ch in enumerate(s):
            if ch.isdigit():
                cur_num = cur_num * 10 + int(ch)
            if ch in "+-/*" or index == len(s)-1:
                if previous_operator == "+":
                    eval_val += last_num 
                    last_num = cur_num
                elif previous_operator=="-":
                    eval_val -= last_num 
                    last_num = cur_num
                elif previous_operator == "*":
                    last_num = last_num * cur_num
                else:
                    last_num = math.ceil(last_num/cur_num) 
                previous_operator=ch
                cur_num=0

        return last_num + eval_val

o = Solution()
print(o.basic_calculator(" 2 + 3 * 4 + 3 "))
```
