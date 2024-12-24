### Morris Traversal

Morris traversal is an **in-order traversal** algorithm for binary trees that **avoids recursion and stack**. Instead, it modifies the tree temporarily (without using extra space) by creating **threaded binary trees**. This technique is very efficient in terms of space complexity because it runs in **O(n)** time and **O(1)** space.

### Key Concepts:

1. **Threaded Binary Tree**: This is a binary tree in which the **null pointers** of the nodes (i.e., the left or right child pointers) are used to point to the **in-order predecessor** or **in-order successor** of the node. This allows the tree to be traversed without using extra space.

2. **Morris In-Order Traversal**: In the typical recursive in-order traversal, we traverse the left subtree, visit the node, and then traverse the right subtree. Morris traversal achieves the same result, but it does so without using the stack or recursion. Instead, it temporarily manipulates the tree structure by creating temporary links (or threads).

### Steps of Morris In-Order Traversal:

1. **Start at the root** of the tree.
2. **While the current node is not null**:
   - If the current node has no left child:
     - Visit the current node (print it, or store the value).
     - Move to the right child.
   - Otherwise, find the **in-order predecessor** of the current node:
     - The in-order predecessor is the **rightmost node** of the left subtree (or the left child of the current node). 
     - If the in-order predecessor's right child is `None`, create a **thread** by linking the predecessor's right child to the current node. Then, move to the left child of the current node.
     - If the in-order predecessor's right child is already the current node (i.e., the thread already exists), it means we've finished traversing the left subtree, so:
       - Visit the current node.
       - Break the thread (set the right child of the predecessor to `None`).
       - Move to the right child of the current node.

### Pseudocode:
Here’s the basic idea in pseudocode for Morris Traversal (In-Order):

```python
def morris_inorder(root):
    current = root
    while current is not None:
        if current.left is None:
            # Visit the current node (print, or store value)
            print(current.val)
            current = current.right
        else:
            # Find the in-order predecessor
            predecessor = current.left
            while predecessor.right is not None and predecessor.right is not current:
                predecessor = predecessor.right

            if predecessor.right is None:
                # Create a thread to the current node
                predecessor.right = current
                current = current.left
            else:
                # Thread already exists, visit the node
                predecessor.right = None
                print(current.val)
                current = current.right
```

### Example Walkthrough:

Consider the following binary tree:

```
         1
        / \
       2   3
      / \
     4   5
```

The goal is to print the values in in-order sequence: `4, 2, 5, 1, 3`.

#### Step-by-Step Morris Traversal:

1. **Start at root (1)**: The left child of 1 is 2. So, we find the in-order predecessor of 1, which is 5 (rightmost child of 2).
2. Set the right child of 5 to 1 (create the thread), and move the current node to 2.
3. **At node 2**: The left child is 4. The in-order predecessor of 2 is 4 (rightmost child of 4).
4. Set the right child of 4 to 2 (create the thread), and move the current node to 4.
5. **At node 4**: It has no left child, so we visit it and print 4. Move to the right (which is the thread pointing to 2).
6. **At node 2**: We visit it and print 2. The thread from 4 to 2 is removed, and we move to the right child, which is 5.
7. **At node 5**: It has no left child, so we visit it and print 5. Move to the right (which is the thread pointing to 1).
8. **At node 1**: We visit it and print 1. The thread from 5 to 1 is removed, and we move to the right child, which is 3.
9. **At node 3**: It has no left child, so we visit it and print 3. Move to the right, which is `None`, so the traversal ends.

Final Output: `4, 2, 5, 1, 3`.

### Advantages of Morris Traversal:

- **Space Complexity**: The main advantage is that it does not require additional space for a stack or recursion, so it uses **O(1)** space.
- **Time Complexity**: It still runs in **O(n)** time, where `n` is the number of nodes, because each node is visited twice (once during the thread creation and once when it is printed).

### Disadvantages:

- **Modifying the Tree**: The tree is modified during the traversal by adding and removing threads. While these changes are temporary, if the tree needs to remain unchanged for later operations, this could be a problem.
- **Complexity**: Morris traversal is more complex to understand and implement compared to the more straightforward recursive or stack-based approaches.

### Code Example for Morris Traversal (In-Order):

```python
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

def morris_inorder(root):
    current = root
    while current:
        if current.left is None:
            print(current.val, end=" ")
            current = current.right
        else:
            # Find the rightmost node of the left subtree
            predecessor = current.left
            while predecessor.right is not None and predecessor.right is not current:
                predecessor = predecessor.right

            if predecessor.right is None:
                # Establish a thread to the current node
                predecessor.right = current
                current = current.left
            else:
                # Visit the current node
                predecessor.right = None
                print(current.val, end=" ")
                current = current.right

# Example Usage
root = TreeNode(1)
root.left = TreeNode(2)
root.right = TreeNode(3)
root.left.left = TreeNode(4)
root.left.right = TreeNode(5)

print("Morris In-order Traversal:")
morris_inorder(root)
```

### Output:

```
Morris In-order Traversal:
4 2 5 1 3
```
### Leetcode: sum root to leaf
```python
class Solution:
    def sumNumbers(self, root: TreeNode) -> int:
        root_to_leaf = curr_number = 0

        while root:
            # If there is a left child,
            # then compute the predecessor.
            # If there is no link predecessor.right = root --> set it.
            # If there is a link predecessor.right = root --> break it.
            if root.left:
                # Predecessor node is one step to the left
                # and then to the right till you can.
                predecessor = root.left
                steps = 1
                while predecessor.right and predecessor.right is not root:
                    predecessor = predecessor.right
                    steps += 1

                # Set link predecessor.right = root
                # and go to explore the left subtree
                if predecessor.right is None:
                    curr_number = curr_number * 10 + root.val
                    predecessor.right = root
                    root = root.left
                # Break the link predecessor.right = root
                # Once the link is broken,
                # it's time to change subtree and go to the right
                else:
                    # If you're on the leaf, update the sum
                    if predecessor.left is None:
                        root_to_leaf += curr_number
                    # This part of tree is explored, backtrack
                    for _ in range(steps):
                        curr_number //= 10
                    predecessor.right = None
                    root = root.right

            # If there is no left child
            # then just go right.
            else:
                curr_number = curr_number * 10 + root.val
                # if you're on the leaf, update the sum
                if root.right is None:
                    root_to_leaf += curr_number
                root = root.right

        return root_to_leaf
```
### Conclusion:

Morris Traversal is an efficient way to traverse a binary tree without using extra space for recursion or a stack. 
By temporarily modifying the tree structure using threads, it enables in-order traversal in **O(n)** time with **O(1)** space complexity. 
However, it requires careful management of the tree's pointers and may not be suitable if the tree structure cannot be altered.
