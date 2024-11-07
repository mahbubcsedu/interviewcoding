### Bipartite Graphs: A Complete Guide with Example and Practice Problems

A **bipartite graph** is a type of graph where we can split the vertices into two distinct groups such that no two vertices within the same group are connected. This is useful in scenarios like scheduling problems, matching problems, and scenarios that involve grouping and non-overlapping criteria.

---

### Definition and Properties of Bipartite Graphs

A graph \(G = (V, E)\) is **bipartite** if we can partition the set of vertices \(V\) into two disjoint sets, say \(X\) and \(Y\), so that every edge connects a vertex in \(X\) with a vertex in \(Y\). No edge connects two vertices within the same set.

#### Key Properties
1. **Two-Colorable**: A graph is bipartite if we can color it with two colors such that no two adjacent vertices have the same color.
2. **No Odd-Length Cycles**: A graph is bipartite if and only if it does not contain any cycles of odd length.

#### Applications
- **Matching problems**: Job assignment, resource allocation, network flow.
- **Scheduling**: Task and resource distribution.
- **Social networks**: Relationships like people and interests, actors and movies, etc.

---

### Example Problem: Check if a Graph is Bipartite

Let’s consider a graph where you need to determine if it’s bipartite. The graph is represented as an adjacency list.

#### Problem Statement
Given an undirected graph, determine if it is bipartite. If we can divide the vertices into two groups such that no two adjacent vertices are in the same group, the graph is bipartite.

#### Approach

We can solve this problem using **Breadth-First Search (BFS)** or **Depth-First Search (DFS)** by attempting to color the graph with two colors:
1. Start from any node, assign it a color (e.g., color 0).
2. For each adjacent node, assign the opposite color. If an adjacent node is already colored with the same color, the graph is not bipartite.
3. Repeat the process for all connected components in the graph.

### Code Example: Checking Bipartiteness Using BFS

```python
from collections import deque

def is_bipartite(graph):
    # Color array to store color assignments
    # -1 means no color assigned yet
    color = [-1] * len(graph)

    for start in range(len(graph)):
        # If the node is uncolored, perform BFS
        if color[start] == -1:
            queue = deque([start])
            color[start] = 0  # Start coloring with color 0
            
            while queue:
                node = queue.popleft()
                for neighbor in graph[node]:
                    # If neighbor has the same color as the current node, it's not bipartite
                    if color[neighbor] == color[node]:
                        return False
                    # If neighbor hasn't been colored, color it with the opposite color
                    if color[neighbor] == -1:
                        color[neighbor] = 1 - color[node]
                        queue.append(neighbor)
    
    return True  # All nodes processed without conflicts

# Example usage
graph = [
    [1, 3],    # Node 0 connected to nodes 1 and 3
    [0, 2],    # Node 1 connected to nodes 0 and 2
    [1, 3],    # Node 2 connected to nodes 1 and 3
    [0, 2]     # Node 3 connected to nodes 0 and 2
]

print("Is the graph bipartite?", is_bipartite(graph))
```

#### Explanation
In this example:
- We use a `color` array initialized to -1 for each node.
- Starting from an uncolored node, we perform BFS, coloring nodes alternately (0 and 1).
- If we detect any adjacent nodes with the same color, we return `False`, indicating the graph is not bipartite.

---

### Visualizing the Graph Coloring

Consider this small graph:

```
   0 —— 1
   |    |
   3 —— 2
```

The BFS will color nodes alternately. Starting from node `0` with color `0`, nodes `1` and `3` will be colored `1`, and node `2` will be colored `0`.

---

### Practice Problems on Bipartite Graphs

1. **[Is Graph Bipartite? (LeetCode #785)](https://leetcode.com/problems/is-graph-bipartite/)**
   - **Description**: Given an adjacency list of a graph, determine if it’s bipartite.
   - **Approach**: Use BFS or DFS to color nodes and check if adjacent nodes have opposite colors.

2. **[Possible Bipartition (LeetCode #886)](https://leetcode.com/problems/possible-bipartition/)**
   - **Description**: Given a set of `n` people with dislikes, determine if it’s possible to split them into two groups where no two people in the same group dislike each other.
   - **Approach**: Treat each person as a node and dislikes as edges, then use BFS to check for bipartiteness.

3. **[Maximum Bipartite Matching (GeeksforGeeks)](https://www.geeksforgeeks.org/maximum-bipartite-matching/)**
   - **Description**: Given a bipartite graph, find the maximum matching (e.g., job assignments).
   - **Approach**: Use augmenting paths and a DFS-based approach to find the maximum matching.

4. **[Two Sets (Codeforces #1092D2)](https://codeforces.com/problemset/problem/1092/D2)**
   - **Description**: Partition a sequence into two sets where each set can satisfy specific conditions.
   - **Approach**: Formulate as a bipartite problem where you need to check certain properties across pairs.

5. **[Bipartite Graphs and DFS (LeetCode Discuss)](https://leetcode.com/discuss/interview-question/237247/)**
   - **Description**: Explore advanced graph coloring techniques using DFS for more complex bipartite checks.
   - **Approach**: Implement DFS to solve custom interview questions involving bipartite structures.

6. **[Campus Bikes II (LeetCode #1066)](https://leetcode.com/problems/campus-bikes-ii/)**
   - **Description**: Given workers and bikes on a campus, find the minimum sum of distances to assign bikes.
   - **Approach**: Use a bipartite matching and optimization techniques to assign bikes.

7. **[Check if a Graph is Bipartite (HackerRank)](https://www.hackerrank.com/challenges/bipartite-graph)**
   - **Description**: Implement a solution to check if a graph provided in input is bipartite.
   - **Approach**: Apply BFS or DFS for bipartite verification.

---

### Tips for Working with Bipartite Graphs

1. **Always Check All Nodes**: For disconnected graphs, ensure every component is checked, as one unconnected component may still make the whole graph non-bipartite.
2. **Use BFS for Consistent Coloring**: BFS is often preferred for coloring since it explores level by level, which aligns with alternating color requirements.
3. **DFS Also Works**: DFS can also work, especially in recursive solutions where you alternate colors at each depth.
4. **Practice with Matching Problems**: Many advanced bipartite graph problems involve matching, so explore augmenting paths and Hungarian algorithms.

By solving these problems, you’ll gain a solid understanding of bipartite graphs and their applications in graph theory and real-world scenarios. Happy coding!
