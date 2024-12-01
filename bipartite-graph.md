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

The **Job Scheduling Problem** can be solved using a **Maximum Bipartite Matching** algorithm when framed as a bipartite graph. Here's a walkthrough of the approach:

---

### **Problem Statement**
Given:
1. A set of jobs \(J = \{j_1, j_2, ..., j_n\}\).
2. A set of workers \(W = \{w_1, w_2, ..., w_m\}\).
3. A bipartite graph \(G\), where:
   - An edge exists between \(j_i\) and \(w_k\) if worker \(w_k\) can perform job \(j_i\).

Objective: 
Maximize the number of jobs assigned such that each job is assigned to one worker, and each worker performs at most one job.

---

### **Formulating as a Bipartite Graph**
1. Represent jobs and workers as two disjoint sets of vertices.
2. Create edges between jobs and workers based on compatibility.
3. Solve for the **maximum bipartite matching**.

---

### **Maximum Bipartite Matching Algorithm**

The **Ford-Fulkerson** or **DFS-based Augmented Path** approach is commonly used for bipartite matching:
1. Use a **DFS** to find augmenting paths in the graph.
2. Maintain a `match` array to track assignments.
3. If a worker or job can be reassigned to increase the matching, adjust the `match` accordingly.

---

### **Python Implementation**

Here is a DFS-based approach for the problem:

```python
def bpm(graph, u, visited, match):
    """
    Helper function for DFS-based approach to find augmenting paths.
    :param graph: Adjacency list representing the bipartite graph.
    :param u: Current job.
    :param visited: Array to mark visited workers.
    :param match: Array storing the current match for each worker.
    :return: True if a matching is found.
    """
    for v in graph[u]:  # Iterate over all workers connected to this job
        if not visited[v]:
            visited[v] = True  # Mark worker as visited
            
            # If worker is not matched or can find an alternate job
            if match[v] == -1 or bpm(graph, match[v], visited, match):
                match[v] = u  # Assign job to worker
                return True
    return False

def max_bipartite_matching(jobs, workers, edges):
    """
    Main function to compute maximum bipartite matching.
    :param jobs: List of jobs.
    :param workers: List of workers.
    :param edges: List of edges (job, worker pairs).
    :return: Maximum number of jobs assigned and matching.
    """
    # Build adjacency list for the bipartite graph
    n = len(jobs)
    m = len(workers)
    graph = [[] for _ in range(n)]
    for job, worker in edges:
        graph[job].append(worker)

    # Initialize matching array and count
    match = [-1] * m  # Store which worker is assigned to each job (-1 for unmatched)
    result = 0

    # Try to find a match for every job
    for u in range(n):
        visited = [False] * m
        if bpm(graph, u, visited, match):
            result += 1

    return result, match
```

---

### **Example**

#### Input
- Jobs: `[0, 1, 2]` (indices of jobs).
- Workers: `[0, 1, 2]` (indices of workers).
- Edges: `[(0, 0), (0, 1), (1, 1), (2, 2)]` (job-worker pairs).

#### Output
Maximum number of jobs assigned: `3`.

#### Usage

```python
jobs = [0, 1, 2]
workers = [0, 1, 2]
edges = [(0, 0), (0, 1), (1, 1), (2, 2)]

max_jobs, matching = max_bipartite_matching(jobs, workers, edges)
print("Maximum Jobs Assigned:", max_jobs)
print("Matching:", matching)
```

---

### **Explanation**
1. Build the adjacency list:
   ```
   Job 0: [0, 1]
   Job 1: [1]
   Job 2: [2]
   ```
2. DFS to find augmenting paths:
   - Job 0 → Worker 0.
   - Job 1 → Worker 1.
   - Job 2 → Worker 2.
3. Result: All jobs are matched.

---

### **Complexity**
- **Time Complexity**: $$O(V*E)$$, where \(V\)  is the number of jobs and workers, and \(E\) is the number of edges.
- **Space Complexity**: $$O(V + E)$$ for the adjacency list.

---

This approach ensures an optimal solution for the **Job Scheduling Problem** using **Maximum Bipartite Matching**.

### Maximum number of accepted inviation (leetcode 1820)
```python
class Solution:
    def maximumInvitations(self, grid: List[List[int]]) -> int:

        BOYS, GIRLS = len(grid), len(grid[0])
        matches = {}

        def dfs(boy, visited):
            for girl in range(GIRLS):
                # if boy wants to send invitation to girl or the girl already have not
                # been asked before
                if grid[boy][girl] and girl not in visited:
                    visited.add(girl)
                    # girl is available or if her partner can find alternative, then she can 
                    # go with the current boy
                    if girl not in matches or dfs(matches[girl], visited):
                        matches[girl]=boy
                        return True
            return False
        
        # total = 0
        for boy in range(BOYS):
            dfs(boy, set())
                
        return len(matches)
        
```

### **Kuhn's Matching Algorithm**

Kuhn's algorithm is a classic method used to find the **maximum bipartite matching** in a bipartite graph. It is based on a **DFS-based augmenting path** strategy and works efficiently with an adjacency list representation of the graph.

---

### **Problem Statement**
Given a bipartite graph \( G = (U, V, E) \):
1. \( U \): Set of vertices on one side of the bipartition.
2. \( V \): Set of vertices on the other side.
3. \( E \): Edges connecting \( U \) and \( V \).

The goal is to find the **maximum matching**, i.e., the largest set of edges such that:
- Each vertex in \( U \) and \( V \) is incident to at most one edge in the set.

---

### **Algorithm Description**

1. **Matching Initialization**:
   - Start with an empty matching (no edges initially matched).
   - Maintain a `match` array to store the matched vertex for each vertex in \( V \).

2. **Augmenting Path**:
   - For each unmatched vertex in \( U \), perform a DFS to find an augmenting path.
   - An **augmenting path** is a path that alternates between unmatched and matched edges, starting and ending at unmatched vertices.

3. **Augment the Matching**:
   - If an augmenting path is found, flip the edges (matched ↔ unmatched) along the path to increase the size of the matching.

4. **Repeat**:
   - Continue searching for augmenting paths until no more paths can be found.

5. **Result**:
   - The size of the matching when no more augmenting paths exist is the **maximum bipartite matching**.

---

### **Python Implementation**

Here is the Python implementation of Kuhn's algorithm:

```python
def kuhn(u, visited, match, graph):
    """
    Perform DFS to find an augmenting path.
    :param u: Current vertex in U.
    :param visited: List to mark visited vertices in V.
    :param match: List storing the match for each vertex in V.
    :param graph: Adjacency list of the bipartite graph.
    :return: True if an augmenting path is found.
    """
    for v in graph[u]:
        if not visited[v]:
            visited[v] = True
            # If v is unmatched or its match can find an alternate path
            if match[v] == -1 or kuhn(match[v], visited, match, graph):
                match[v] = u
                return True
    return False

def maximum_bipartite_matching(graph, U, V):
    """
    Find the maximum matching in a bipartite graph using Kuhn's algorithm.
    :param graph: Adjacency list of the bipartite graph.
    :param U: List of vertices in set U.
    :param V: List of vertices in set V.
    :return: The size of the maximum matching.
    """
    match = [-1] * len(V)  # Matches for vertices in V
    result = 0

    for u in U:
        visited = [False] * len(V)
        if kuhn(u, visited, match, graph):
            result += 1

    return result, match
```

---

### **Example Usage**

#### Input
```python
# Bipartite graph with 4 vertices in U and 4 vertices in V
U = [0, 1, 2, 3]
V = [0, 1, 2, 3]

# Edges in the bipartite graph
edges = [
    (0, 0), (0, 1),
    (1, 1), (1, 2),
    (2, 2), (3, 3)
]

# Convert edges to adjacency list
graph = {u: [] for u in U}
for u, v in edges:
    graph[u].append(v)

# Compute maximum bipartite matching
max_matching, matches = maximum_bipartite_matching(graph, U, V)
print("Maximum Matching:", max_matching)
print("Matches:", matches)
```

#### Output
```
Maximum Matching: 4
Matches: [0, 1, 2, 3]
```

---

### **Example Walkthrough**

#### Graph Representation
- \( U = \{0, 1, 2, 3\} \), \( V = \{0, 1, 2, 3\} \).
- Edges: \( (0, 0), (0, 1), (1, 1), (1, 2), (2, 2), (3, 3) \).

#### Steps:
1. Start with an empty matching.
2. For \( u = 0 \), find an augmenting path \( 0 → 0 \). Update \( match[0] = 0 \).
3. For \( u = 1 \), find an augmenting path \( 1 → 1 \). Update \( match[1] = 1 \).
4. For \( u = 2 \), find an augmenting path \( 2 → 2 \). Update \( match[2] = 2 \).
5. For \( u = 3 \), find an augmenting path \( 3 → 3 \). Update \( match[3] = 3 \).

Result:
- Maximum matching size = 4.
- Matches: Each vertex in \( V \) is matched with a unique vertex in \( U \).

---

### **Complexity**

- **Time Complexity**: \(O(V \cdot E)\), where \(V\) is the number of vertices in \(U\) and \(V\), and \(E\) is the number of edges.
  - Each DFS runs in \(O(E)\).
  - At most \(O(V)\) DFS calls.

- **Space Complexity**: \(O(V + E)\) for the adjacency list and visited array.

---

### **Applications**
1. Job scheduling problems.
2. Assigning tasks in resource allocation.
3. Matching problems in networks, markets, and logistics.

Kuhn's algorithm is simple and efficient for bipartite matching and forms the foundation for more advanced methods like the **Hopcroft-Karp algorithm**.
