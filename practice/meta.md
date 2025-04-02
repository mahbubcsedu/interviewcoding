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
