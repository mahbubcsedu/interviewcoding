### Hamiltonian Path Problem: Concept

A **Hamiltonian Path** in a graph is a path that visits each vertex exactly once. If the path is a cycle (starts and ends at the same vertex), it is called a **Hamiltonian Cycle**.

#### Key Properties:
1. **Directed Graphs**: A Hamiltonian path follows the direction of edges.
2. **Undirected Graphs**: Edges can be traversed in either direction.
3. **Comparison with Eulerian Path**:
   - A Hamiltonian path focuses on vertices.
   - An Eulerian path focuses on edges.

The problem is **NP-complete**, meaning no polynomial-time solution is known for arbitrary graphs.

---

### Algorithms to Solve Hamiltonian Path Problem

1. **Backtracking**:
   - Start at a vertex and recursively explore all possible paths.
   - Maintain a visited array to track visited vertices.
   - Check for the base condition when all vertices have been visited.
   - Time complexity: \(O(V!)\) (exponential).

   **Pseudocode**:
   ```python
   def is_hamiltonian_path(graph, path, visited, current):
       if len(path) == len(graph):
           return True  # Path includes all vertices.

       for neighbor in graph[current]:
           if not visited[neighbor]:
               visited[neighbor] = True
               path.append(neighbor)
               if is_hamiltonian_path(graph, path, visited, neighbor):
                   return True
               path.pop()
               visited[neighbor] = False

       return False
   ```

2. **Dynamic Programming with Bitmasking** (for small graphs):
   - Use a bitmask to represent visited vertices.
   - Define `dp[mask][v]` as whether it's possible to visit all vertices in `mask` ending at vertex `v`.
   - Transition: Update states by adding new vertices to the set.
   - Time complexity: \(O(2^V \times V^2)\).

3. **Reduction to SAT/CSP**:
   - Encode the problem as a constraint satisfaction problem.
   - Solve using SAT solvers for practical-sized graphs.

---

### Problems Using Hamiltonian Path

1. **Graph Connectivity Problems**:
   - Determine if there exists a path that visits every vertex.
   - Variants include finding such a path in weighted graphs for minimum cost.

2. **Sequence Reconstruction**:
   - Check if a sequence can be reconstructed uniquely based on subsequences.

3. **Traveling Salesman Problem (TSP)**:
   - TSP is a weighted version of the Hamiltonian Cycle problem.

4. **Hamiltonian Path in Grid**:
   - Given a grid of points, find a path visiting all points exactly once.

---

### Practice Problems

1. **LeetCode 980: Unique Paths III**  
   - Find the number of unique paths that visit all non-obstacle cells exactly once.  
   - [Link to problem](https://leetcode.com/problems/unique-paths-iii/)

2. **Hamiltonian Path Checker** (Custom Problem):  
   - Given an adjacency matrix, check if a Hamiltonian path exists.

3. **Graph Valid Tree (LeetCode 261)**:
   - Check if a graph forms a valid tree (can involve Hamiltonian concepts) but does not need to include that to solve problem as the solution can be done with less cost. See the solution bellow

4. **Minimum Hamiltonian Path in a Weighted Graph**:
   - Solve using backtracking or dynamic programming.

Let me know if you want detailed solutions or explanations for any of these!

## Graph Valid Tree
The **Graph Valid Tree** problem cannot be directly solved using Hamiltonian concepts efficiently because checking for a Hamiltonian Path is an NP-complete problem. However, understanding Hamiltonian paths provides useful insights when validating paths in a graph.

To solve **Graph Valid Tree**, we can instead rely on simpler graph properties:

### Problem Description:
Determine if a graph forms a valid tree:
1. The graph must be connected (all nodes are reachable from any other node).
2. The graph must be acyclic (no cycles).

These properties ensure that the graph is a spanning tree.

---

### Solution Using Graph Properties
1. **Connectivity**: Perform a depth-first search (DFS) or breadth-first search (BFS) to ensure all nodes are reachable.
2. **Acyclicity**: Ensure the graph has exactly \(n - 1\) edges for \(n\) nodes (a necessary condition for trees).

---

### Code Implementation (Efficient Approach)

Here’s how to solve the problem efficiently using DFS:

```python
def valid_tree(n, edges):
    # If the number of edges is not exactly n - 1, it cannot be a tree
    if len(edges) != n - 1:
        return False

    # Build the graph as an adjacency list
    graph = {i: [] for i in range(n)}
    for u, v in edges:
        graph[u].append(v)
        graph[v].append(u)

    # Perform DFS to check connectivity and detect cycles
    visited = set()

    def dfs(node, parent):
        if node in visited:
            return False  # Cycle detected
        visited.add(node)
        for neighbor in graph[node]:
            if neighbor == parent:
                continue  # Skip the edge to the parent
            if not dfs(neighbor, node):
                return False
        return True

    # Start DFS from node 0
    if not dfs(0, -1):
        return False

    # Check if all nodes are visited (ensures connectivity)
    return len(visited) == n

# Example Usage
n = 5
edges = [[0, 1], [0, 2], [0, 3], [1, 4]]
print(valid_tree(n, edges))  # Output: True
```

---

### Why Hamiltonian Concepts Aren’t Ideal Here

1. **Hamiltonian Path Focuses on Visiting All Vertices**: While a valid tree does involve visiting all vertices in some sense, a Hamiltonian path enforces visiting all vertices in a specific sequence, which is unnecessary for this problem.
2. **Efficiency**: Checking for a Hamiltonian path requires exploring all permutations or exponential backtracking, while validating a tree only requires \(O(V + E)\) operations.

If you'd like, I can demonstrate how to reduce Graph Valid Tree to a Hamiltonian Path problem, but it won't be practical for large graphs. Would you like that?

## LeetCode 980: Unique Paths III
**LeetCode 980: Unique Paths III** is a problem that involves finding the number of unique paths that visit all non-obstacle cells in a grid exactly once, starting from a specific cell and ending at another specific cell.

---

### Problem Analysis

1. **Input**:  
   A grid where:
   - `0` represents an empty cell.
   - `-1` represents an obstacle.
   - `1` represents the starting cell.
   - `2` represents the ending cell.

2. **Output**:  
   The number of unique paths from the start cell to the end cell that visit all empty cells exactly once.

3. **Constraints**:
   - You must visit all non-obstacle cells.
   - Each cell can only be visited once in a single path.

---

### Approach

We can solve this problem using **backtracking**:

1. **Count Non-Obstacle Cells**: Count the total number of cells that must be visited.
2. **Find Start and End Points**: Identify the starting (`1`) and ending (`2`) positions.
3. **Backtracking Function**:
   - Start from the `1` cell.
   - Move to all possible neighbors (up, down, left, right).
   - Mark cells as visited during exploration and unmark them during backtracking.
   - Stop when reaching the end cell (`2`) after visiting all required cells.

4. **Base Cases**:
   - If we reach the ending cell and all empty cells have been visited, count this as a valid path.
   - If we move out of bounds, hit an obstacle, or revisit a cell, terminate the path.

---

### Code Implementation

Here’s the Python code for the solution:

```python
def uniquePathsIII(grid):
    rows, cols = len(grid), len(grid[0])
    empty_cells = 0
    start_x = start_y = end_x = end_y = 0

    # Count empty cells and locate start and end points
    for i in range(rows):
        for j in range(cols):
            if grid[i][j] == 0:
                empty_cells += 1
            elif grid[i][j] == 1:
                start_x, start_y = i, j
            elif grid[i][j] == 2:
                end_x, end_y = i, j

    def backtrack(x, y, remaining):
        # If we reach the end cell with all cells visited
        if x == end_x and y == end_y:
            return 1 if remaining == 0 else 0

        # Mark the current cell as visited
        grid[x][y] = -1
        paths = 0

        # Explore neighbors
        for dx, dy in [(-1, 0), (1, 0), (0, -1), (0, 1)]:
            nx, ny = x + dx, y + dy
            if 0 <= nx < rows and 0 <= ny < cols and grid[nx][ny] in (0, 2):
                paths += backtrack(nx, ny, remaining - 1)

        # Backtrack (unmark the cell)
        grid[x][y] = 0

        return paths

    # Start the backtracking from the starting cell
    return backtrack(start_x, start_y, empty_cells + 1)

# Example Usage
grid = [[1, 0, 0, 0], [0, 0, 0, 0], [0, 0, 2, -1]]
print(uniquePathsIII(grid))  # Output: 2
```

---

### Explanation of the Code

1. **Grid Traversal**:
   - Start from the cell marked `1`.
   - Traverse through valid cells (either `0` or `2`).

2. **Recursive Backtracking**:
   - Each recursive call moves to a neighboring cell and decrements the remaining cells to visit.
   - If all conditions are met at the end cell, increment the path count.

3. **Backtracking**:
   - After exploring all possibilities from a cell, restore its state to allow exploration of other paths.

---

### Complexity

- **Time Complexity**: \(O(4^n)\), where \(n\) is the number of empty cells. This is due to exploring 4 directions at each step.
- **Space Complexity**: \(O(n)\) for the recursive call stack.

---

Let me know if you'd like further explanation or examples for this problem!
