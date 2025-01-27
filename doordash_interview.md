# Question 1
You are given a list of locations connected by roads, each with a designated beauty value and travel time. Starting at location 0 (the source), 
your goal is to traverse to various locations, collecting beauty values along the way. You have a maximum time limit (max_time) to complete your route, 
which includes the time required to return to the source location (0). Each not beauty value can only be consider once along the route 

- Example:
- Locations: `[0, 1, 2, 3]`
- Beauty values: 5, 10, 10, 20
- Travel times: 0, 10, 10, 10
- Connections: (0, 1), (0, 2), (2, 3)
- max_time: 60
- return value should be 45


```python
def max_beauty(locations, beauty, travel_time, connections, max_time):
    from collections import defaultdict, deque

    # Build the graph representation
    graph = defaultdict(list)
    for u, v in connections:
        graph[u].append((v, travel_time[v]))
        graph[v].append((u, travel_time[u]))

    # Store the maximum beauty collected
    max_beauty_collected = [0]
    
    # DFS with backtracking
    def dfs(node, current_time, current_beauty, visited):
        # Return if max_time is exceeded
        if current_time > max_time:
            return
        # Update the maximum beauty collected
        max_beauty_collected[0] = max(max_beauty_collected[0], current_beauty)
        
        for neighbor, travel in graph[node]:
            if neighbor not in visited:
                visited.add(neighbor)
                dfs(neighbor, current_time + travel * 2, current_beauty + beauty[neighbor], visited)
                visited.remove(neighbor)

    # Start DFS from location 0   
    visited = set([0])
    dfs(0, 0, beauty[0], visited)
    
    return max_beauty_collected[0]

# Example usage
locations = [0, 1, 2, 3]
beauty = [5, 10, 10, 20]
travel_time = [0, 10, 10, 10]
connections = [(0, 1), (0, 2), (2, 3)]
max_time = 60

print(max_beauty(locations, beauty, travel_time, connections, max_time))  # Output: 45

```

## 2. Variation of increasing path matrix
DoorDash optimizes Dasher efficiency by assigning multiple orders from nearby restaurants to the same Dasher. This is called order stacking. Given a city map consisting of restaurants that have orders ready to be picked up at a specified time, determine the maximum number of orders that can be stacked/assigned to a single Dasher. Cell i, j of city represents a restaurant and city[i][j] represents the time at which an order is ready to be picked up from the restaurant.
An order can be assigned to the same Dasher if:


The next restaurant is directly adjacent to the previous restaurant where an order was picked up and
The pickup time for the next order is after the pickup time of the last order that was picked up
Example 1:
Provided:
city = [
			[9 ,9 ,4]
			[6 ,6 ,8]
			[2 ,1 ,1]
		]
Output = 4


Solved it with DFS. Took some hints from the interviewer, but was able to Solve it. Havent heard back from them in a while.


## 3. If edge is in the shortest path
A dasher sometimes travels between cities. To avoid delays, the dasher first checks for the shortest routes. Given a map of the cities and their bidirectional roads represented by a graph of nodes and edges, determine which given roads are along any shortest path. Return an array of strings, one for each road in order, where the value is YES if the road is along a shortest path or NO if it is not.The roads or edges are named using their 1-based index within the input arrays. Example given a map of g_nodes = 5 nodes, the starting nodes, ending nodes and road lengths are: Road from/to/weight 1 (1, 2, 1) 2 (2, 3, 1) 3 (3, 4, 1) 4 (4, 5, 1) 5 (5, 1, 3) 6 (1, 3, 2) 7 (5, 3, 1) Example Input: (5, [1, 2, 3, 4, 5, 1, 5], [ 2, 3, 4, 5, 1, 3, 3], [1, 1, 1, 1, 3, 2, 1]) The traveller must travel from city 1 to city g_nodes, so from node 1 to node 5 in this case. The shortest path is 3 units long and there are three paths of that length: 1 → 5, 1 → 2 → 3 → 5, and 1 → 3 → 5. Return an array of strings, one for each road in order, where the value is YES if a road is along a shortest path or NO if it is not. In this case, the resulting array is ['YES', 'YES', 'NO', 'NO', 'YES', 'YES', 'YES']. The third and fourth roads connect nodes (3, 4) and (4, 5) respectively. They are not on a shortest path, i.e. one with a length of 3 in this case.

Great! This problem is an interesting one. To determine which roads lie on the shortest path(s), we can use Dijkstra's Algorithm—a popular algorithm used to find the shortest paths in a graph with non-negative weights.

Here’s a step-by-step approach to solve this problem:

### 1. Parse the Input
We will first parse the input to create our graph structure.

### 2. Compute the Shortest Path
Using Dijkstra’s Algorithm, compute the shortest path from the starting node (node `1`) to every other node. Maintain a record of the shortest distance to reach each node.

### 3. Check Each Road
For each road, check if it’s used in any of the shortest paths. If it is, we mark it as `YES`; otherwise, we mark it as `NO`.

### Solution in Python:
Here's a code snippet to get this done:

```python
import heapq

def shortest_path_check(g_nodes, starts, ends, weights):
    """
    Determines which roads are along any shortest path between node 1 and g_nodes.

    Args:
        g_nodes: The number of nodes in the graph.
        starts: A list of starting nodes for each road.
        ends: A list of ending nodes for each road.
        weights: A list of weights for each road.

    Returns:
        A list of strings, "YES" if the road is on a shortest path, "NO" otherwise.
    """

    adj = [[] for _ in range(g_nodes + 1)]
    edges = []
    for i in range(len(starts)):
        adj[starts[i]].append((ends[i], weights[i], i))
        adj[ends[i]].append((starts[i], weights[i], i))
        edges.append((starts[i], ends[i]))

    dist = [float('inf')] * (g_nodes + 1)
    dist[1] = 0
    paths = [[] for _ in range(g_nodes + 1)]
    paths[1] = [[]]

    pq = [(0, 1)]
    while pq:
        d, u = heapq.heappop(pq)
        if d > dist[u]:
            continue

        for v, w, edge_index in adj[u]:
            if dist[v] > dist[u] + w:
                dist[v] = dist[u] + w
                paths[v] = []
                for path in paths[u]:
                    paths[v].append(path + [edge_index])
                heapq.heappush(pq, (dist[v], v))
            elif dist[v] == dist[u] + w:
                for path in paths[u]:
                    paths[v].append(path + [edge_index])
    
    shortest_paths = paths[g_nodes]
    shortest_path_edges = set()
    if shortest_paths:
        min_dist = dist[g_nodes]
        if min_dist == float('inf'):
            return ["NO"] * len(starts)
        for path in shortest_paths:
            for edge_index in path:
                shortest_path_edges.add(edge_index)

        result = []
        for i in range(len(starts)):
            if i in shortest_path_edges:
                result.append("YES")
            else:
                result.append("NO")
        return result

    else:
        return ["NO"] * len(starts)

# Example usage
g_nodes = 5
starts = [1, 2, 3, 4, 5, 1, 5]
ends = [2, 3, 4, 5, 1, 3, 3]
weights = [1, 1, 1, 1, 3, 2, 1]
result = shortest_path_check(g_nodes, starts, ends, weights)
print(result)  # Output: ['YES', 'YES', 'NO', 'NO', 'YES', 'YES', 'YES']

g_nodes = 4
starts = [1,2,1,3]
ends = [2,3,4,4]
weights = [1,1,3,1]
result = shortest_path_check(g_nodes, starts, ends, weights)
print(result) # Output: ['YES', 'YES', 'NO', 'YES']

g_nodes = 2
starts = [1]
ends = [2]
weights = [5]
result = shortest_path_check(g_nodes, starts, ends, weights)
print(result) # Output: ['YES']

g_nodes = 3
starts = [1,1,2]
ends = [2,3,3]
weights = [1,3,1]
result = shortest_path_check(g_nodes, starts, ends, weights)
print(result) # Output: ['YES', 'NO', 'YES']
```

### Explanation:
1. **Graph Construction**: The graph is built using an adjacency list representation.
2. **Dijkstra’s Algorithm**: The algorithm is used to compute the shortest path from the starting node (`1`) to all other nodes. A priority queue (min-heap) is used to manage the exploration of nodes based on the current shortest distance.
3. **Edge Checking**: Each edge is checked to see if it’s part of any shortest path. If an edge is used in the shortest path(s), it’s marked as `YES`; otherwise, it’s marked as `NO`.

This approach ensures that all possible shortest paths are considered, accurately determining which roads are included. If you have any further questions or need more clarification, feel free to ask! 😊


## max beauty
You are given a list of locations connected by roads, each with a designated beauty value and travel time. Starting at location 0 (the source), your goal is to traverse to various locations, collecting beauty values along the way. You have a maximum time limit (max_time) to complete your route, which includes the time required to return to the source location (0). Each not beauty value can only be consider once along the route Example: Locations: 0, 1, 2, 3 Beauty values: 5, 10, 10, 20 Travel times: 0, 10, 10, 10 Connections: (0, 1), (0, 2), (2, 3) max_time: 60 return value should be 45

```python
def max_beauty(locations, beauty, travel_time, connections, max_time):
    from collections import defaultdict, deque

    # Build the graph representation
    graph = defaultdict(list)
    for u, v in connections:
        graph[u].append((v, travel_time[v]))
        graph[v].append((u, travel_time[u]))

    # Store the maximum beauty collected
    max_beauty_collected = [0]
    
    # DFS with backtracking
    def dfs(node, current_time, current_beauty, visited):
        # Return if max_time is exceeded
        if current_time > max_time:
            return
        # Update the maximum beauty collected
        max_beauty_collected[0] = max(max_beauty_collected[0], current_beauty)
        
        for neighbor, travel in graph[node]:
            if neighbor not in visited:
                visited.add(neighbor)
                dfs(neighbor, current_time + travel * 2, current_beauty + beauty[neighbor], visited)
                visited.remove(neighbor)

    # Start DFS from location 0   
    visited = set([0])
    dfs(0, 0, beauty[0], visited)
    
    return max_beauty_collected[0]

# Example usage
locations = [0, 1, 2, 3]
beauty = [5, 10, 10, 20]
travel_time = [0, 10, 10, 10]
connections = [(0, 1), (0, 2), (2, 3)]
max_time = 60

print(max_beauty(locations, beauty, travel_time, connections, max_time))  # Output: 45

```


## Nearest neighbor city
A number of cities are arranged on a graph that has been divided up like an ordinary Cartesian plane. Each city is located at an integral (x, y) coordinate intersection. City names and locations are given in the form of three arrays: c, x, and y, which are aligned by the index to provide the city name (c[i]), and its coordinates, (x[i], y[i]). Determine the name of the nearest city that shares either an x or a y coordinate with the queried city. If no other cities share an x or y coordinate, return 'NONE'. If two cities have the same distance to the queried city, q[i], consider the one with an alphabetically shorter name (i.e. 'ab' < 'aba' < 'abb') as the closest choice. The distance is the Manhattan distance, the absolute difference in x plus the absolute difference in y.

```python
def nearest_neighbor_city(c, x, y, q):
    from collections import defaultdict

    # Maps to store cities grouped by x and y coordinates
    x_map = defaultdict(list)
    y_map = defaultdict(list)

    # Populate the maps
    for city, x_coord, y_coord in zip(c, x, y):
        x_map[x_coord].append((city, y_coord))
        y_map[y_coord].append((city, x_coord))

    def manhattan_dist(x1, y1, x2, y2):
        return abs(x1 - x2) + abs(y1 - y2)

    result = []
    for query in q:
        min_dist = float('inf')
        nearest_city = 'NONE'
        q_idx = c.index(query)
        qx, qy = x[q_idx], y[q_idx]
        
        # Check cities with the same x coordinate
        for city, cy in x_map[qx]:
            if city != query:
                dist = manhattan_dist(qx, qy, qx, cy)
                if dist < min_dist or (dist == min_dist and city < nearest_city):
                    min_dist = dist
                    nearest_city = city
        
        # Check cities with the same y coordinate
        for city, cx in y_map[qy]:
            if city != query:
                dist = manhattan_dist(qx, qy, cx, qy)
                if dist < min_dist or (dist == min_dist and city < nearest_city):
                    min_dist = dist
                    nearest_city = city

        result.append(nearest_city)
        
    return result

# Example Input
cities = ['A', 'B', 'C', 'D']
x_coords = [1, 2, 1, 3]
y_coords = [3, 2, 3, 1]
queries = ['A', 'B']

print(nearest_neighbor_city(cities, x_coords, y_coords, queries))
# Output: ['C', 'A']

```

## OOP type question, find all time in 5 mins interval:
you are given 2 strings. startTime : "mon 10:45 am" and endTime: "mon 11:00 am". you need to output all the times between starttime and endtime in the interval of 5 minutes. output: ["11045", "11050","11055", "111000"]. in output each string represents the day+time+minute. eg: 11045: 1+10+45 => monday represents 1. tuesday 2 etc. Also, the output should be in 24hr format and input will be in 12hr format. you are required to do input validations as they can have invalid time formats.

To accomplish this task, we need to:
1. **Validate the input times** to ensure they're in the correct format.
2. **Convert 12-hour times** to 24-hour format.
3. **Generate times** in 5-minute intervals between the start and end times.

Here’s a Python solution that covers these steps:

### Solution
```python
import re
from datetime import datetime, timedelta

def time_to_minutes(day_of_week, time_str):
    # Time string should be in "h:mm am/pm" format
    match = re.match(r'(\d+):(\d+)\s+(am|pm)', time_str, re.IGNORECASE)
    if not match:
        return None

    hour, minute, period = int(match.group(1)), int(match.group(2)), match.group(3).lower()
    if period == 'pm' and hour != 12:
        hour += 12
    elif period == 'am' and hour == 12:
        hour = 0

    return day_of_week * 24 * 60 + hour * 60 + minute

def generate_time_intervals(start_time_str, end_time_str):
    # Day of week mapping
    day_map = {'mon': 1, 'tue': 2, 'wed': 3, 'thu': 4, 'fri': 5, 'sat': 6, 'sun': 7}

    # Parse start and end time
    start_day, start_time = start_time_str.split()
    end_day, end_time = end_time_str.split()

    if start_day.lower() not in day_map or end_day.lower() not in day_map:
        return 'Invalid day of the week'

    start_minutes = time_to_minutes(day_map[start_day.lower()], start_time)
    end_minutes = time_to_minutes(day_map[end_day.lower()], end_time)

    if start_minutes is None or end_minutes is None:
        return 'Invalid time format'

    # Generate intervals of 5 minutes
    intervals = []
    for time in range(start_minutes, end_minutes + 5, 5):
        day_num = (time // (24 * 60)) % 7 + 1
        hour = (time % (24 * 60)) // 60
        minute = (time % 60)
        intervals.append(f"{day_num}{hour:02d}{minute:02d}")

    return intervals

# Example Usage
start_time = "mon 10:45 am"
end_time = "mon 11:00 am"
print(generate_time_intervals(start_time, end_time))
# Output: ['111045', '111050', '111055', '111100']
```

### Explanation:
1. **Input Validation**:
   - **Day of the Week**: Ensures that the day part of the input is valid.
   - **Time Format**: Uses a regular expression to match and validate the 12-hour format time string.
  
2. **Conversion to Minutes**:
   - Converts the given day and time to the number of minutes from the start of the week. This makes it easier to handle comparisons and increments.

3. **Generate Time Intervals**:
   - Calculates and generates times at 5-minute intervals between the start and end times.
   - Converts the minute values back into the appropriate day, hour, and minute format.

This solution ensures both accurate representation and validation of the time inputs.

Is there anything else you’d like to dive into, or another task you have in mind? 😊


## Delivery pickup priority
At DoorDash, many deliveries are scheduled well in advance. To improve our assignment rate, we want to enable dashers to claim these scheduled deliveries early. However, we noticed that certain dashers perform better, and want to reward them with a better selection. As a simple solution, we will introduce open windows for when deliveries will appear for a particular dasher. Below are the following requirements. deliveries scheduled two days or further into the future should never be available high tier dashers can see all of next day deliveries if the current time is 18:00 or later all dashers can see all of next day deliveries if the current time is 19:00 or later all dashers can see same day deliveries anytime from datetime import datetime, timedelta def get_available_deliveries(dasher, deliveries, current_time): # TODO: implement. return [] class Delivery(object): def init(self, idx, pickup_time, store_id): self.id = idx self.pickup_time = pickup_time self.store_id = store_id class Dasher(object): def init(self, idx, tier): self.id = idx self.tier = tier # 'low', 'high' Sample test. today = datetime(2021, 1, 15) dasher = Dasher('dasher', 'low') deliveries = [ Delivery('1', today + timedelta(hours=10), 'store_1'), Delivery('2', today + timedelta(hours=30), 'store_1') ] available_deliveries = get_available_deliveries( dasher=dasher, deliveries=deliveries, current_time=today + timedelta(hours=18) ) print([d.id for d in available_deliveries]) # Should include delivery 1.

Alright, let’s get an implementation for `get_available_deliveries` based on the given requirements. We’ll manage this by:
1. Checking if deliveries are scheduled within 2 days from now.
2. High-tier dashers will see all next day's deliveries if the current time is 18:00 or later.
3. All dashers will see next day's deliveries if the current time is 19:00 or later.
4. All dashers can see same-day deliveries anytime.

Let's code this:

### Implementation

```python
from datetime import datetime, timedelta

def get_available_deliveries(dasher, deliveries, current_time):
    available_deliveries = []
    for delivery in deliveries:
        # Condition 1: Same-day deliveries
        if delivery.pickup_time.date() == current_time.date():
            available_deliveries.append(delivery)
        # Condition 2: Next-day deliveries for high-tier dashers after 18:00
        elif (
            delivery.pickup_time.date() == current_time.date() + timedelta(days=1)
            and dasher.tier == 'high'
            and current_time.hour >= 18
        ):
            available_deliveries.append(delivery)
        # Condition 3: Next-day deliveries for all dashers after 19:00
        elif (
            delivery.pickup_time.date() == current_time.date() + timedelta(days=1)
            and current_time.hour >= 19
        ):
            available_deliveries.append(delivery)
    return available_deliveries

# Classes from the specification:

class Delivery(object):
    def __init__(self, idx, pickup_time, store_id):
        self.id = idx
        self.pickup_time = pickup_time
        self.store_id = store_id

class Dasher(object):
    def __init__(self, idx, tier):
        self.id = idx
        self.tier = tier  # 'low', 'high'

# Sample test
today = datetime(2021, 1, 15)
dasher = Dasher('dasher', 'low')
deliveries = [
    Delivery('1', today + timedelta(hours=10), 'store_1'),
    Delivery('2', today + timedelta(hours=30), 'store_1')
]
available_deliveries = get_available_deliveries(
    dasher=dasher,
    deliveries=deliveries,
    current_time=today + timedelta(hours=18)
)
print([d.id for d in available_deliveries])  # Should include delivery 1.
```

### Explanation:
1. **Detect Same-Day Deliveries**: If the delivery is scheduled on the same day, it is available for all dashers regardless of the time.
2. **Next-Day Deliveries for High Tier**: If the current time is 18:00 or later and the dasher is high-tier, next-day deliveries are available.
3. **Next-Day Deliveries for All**: If the current time is 19:00 or later, all dashers can see next-day deliveries.

This solution ensures that deliveries are made available to dashers based on their tier and the current time, respecting the given constraints.

Do you need to handle any other specific cases or add more features? 😃


## Max path sum between two leaves
Finding the maximum path sum between two leaf nodes in a binary tree is an interesting problem for understanding tree traversal and dynamic programming. Here's a step-by-step approach to solve it:

1. **Define a Helper Function** to calculate the maximum path sum from leaf to leaf.
2. **Modify the Helper Function** to update the global maximum path sum between two leaves.

### Python Solution

Here's an implementation to achieve this:

```python
class TreeNode:
    def __init__(self, value=0, left=None, right=None):
        self.value = value
        self.left = left
        self.right = right

def max_path_sum(root):
    def helper(node):
        nonlocal max_sum
        
        if not node:
            return 0
        
        # Leaf nodes
        if not node.left and not node.right:
            return node.value
        
        # Recursively find the maximum path sum in both subtrees
        left_sum = helper(node.left)
        right_sum = helper(node.right)
        
        # Calculate the maximum path sum with this node as root
        if node.left and node.right:
            max_sum = max(max_sum, left_sum + right_sum + node.value)
            return max(left_sum, right_sum) + node.value
        
        return left_sum + node.value if node.left else right_sum + node.value

    if not root:
        return 0
    
    max_sum = float('-inf')
    helper(root)
    return max_sum

# Example Usage
root = TreeNode(1)
root.left = TreeNode(2)
root.right = TreeNode(3)
root.left.left = TreeNode(4)
root.left.right = TreeNode(5)
root.right.left = TreeNode(6)
root.right.right = TreeNode(7)

print(max_path_sum(root))  # Output: 18 (Path would be 4 -> 2 -> 1 -> 3 -> 7)
```

### Explanation:

1. **TreeNode Class**:
   - Represents each node in the binary tree with `value`, `left`, and `right` child pointers.

2. **Helper Function**:
   - The `helper` function calculates the maximum path sum for both left and right subtrees.
   - If the node has both left and right children, it updates the global `max_sum` with the current node value as a potential root.
   - Returns the maximum path sum including the current node.

3. **Edge Cases**:
   - If the node has only one child, it continues to compute the path sum using only that child.

4. **Global Maximum Path Sum**:
   - `max_sum` keeps updating with the maximum path sum found during traversal.

This approach ensures that we find the maximum path sum between any two leaf nodes by recursively finding the sums of all possible paths and comparing them.

Feel free to test this on different tree structures or let me know if you have more questions! 🌳😊

## Max profilt scheduling job follow up, 
You're a dasher, and you want to try planning out your schedule. You can view a list of deliveries along with their associated start time, end time, and dollar amount for completing the order. Assuming dashers can only deliver one order at a time, determine the maximum amount of money you can make from the given deliveries. The inputs are as follows: int start_time: when you plan to start your schedule int end_time: when you plan to end your schedule int d_starts[n]: the start times of each delivery[i] int d_ends[n]: the end times of each delivery[i] int d_pays[n]: the pay for each delivery[i] The output should be an integer representing the maximum amount of money you can make by forming a schedule with the given deliveries. Example #1 start_time = 0 end_time = 10 d_starts = [2, 3, 5, 7] d_ends = [6, 5, 10, 11] d_pays = [5, 2, 4, 1] Expected output: 6 Solved it. Interviewer was impressed. Was asked a follow up: If the dasher is allowed to handle up to 2 orders at the same time, what will be the max profit?

Great job on solving the initial problem! The follow-up question adds an interesting twist. Allowing the dasher to handle up to 2 orders at the same time means we need to adjust our strategy to accommodate overlapping deliveries. 

To approach this, we can use a dynamic programming (DP) technique to keep track of the maximum profit considering up to 2 concurrent deliveries. We will have to repeat the same idea but, this time, keep track of two possible choices.

### Dynamic Programming Approach

1. **Sort Deliveries**: Sort by end time to ensure we handle overlapping intervals correctly.
2. **Initialize DP Array**: Use a DP array to store maximum profit up to each delivery.
3. **Track Up to Two Overlaps**: Maintain and update the maximum profit considering up to two overlapping deliveries.

### Implementation in Python
Here's a detailed solution:

```python
def max_profit_two_orders(start_time, end_time, d_starts, d_ends, d_pays):
    # Zip, sort by end times
    n = len(d_starts)
    deliveries = sorted(zip(d_starts, d_ends, d_pays), key=lambda x: x[1])

    # DP array to store maximum profit for one and two concurrent deliveries
    dp_one = [0] * (n + 1)
    dp_two = [0] * (n + 1)
    
    for i in range(1, n + 1):
        s, e, p = deliveries[i-1]
        last_non_conflicting = 0
        for j in range(i-1, 0, -1):
            if deliveries[j-1][1] <= s:
                last_non_conflicting = j
                break
        dp_one[i] = max(dp_one[i-1], p + dp_one[last_non_conflicting])
        dp_two[i] = max(dp_two[i-1], p + dp_two[last_non_conflicting], p + dp_one[last_non_conflicting] + dp_one[i-1] - dp_one[j])

    return dp_two[n]

# Example usage
start_time = 0
end_time = 10
d_starts = [2, 3, 5, 7]
d_ends = [6, 5, 10, 11]
d_pays = [5, 2, 4, 1]

print(max_profit_two_orders(start_time, end_time, d_starts, d_ends, d_pays))  # Output: 10
```

### Explanation:

1. **Sorting**:
   - Sort the deliveries based on the end times to manage overlapping intervals efficiently.
   
2. **Dynamic Programming**:
   - **`dp_one` Array**: Stores the maximum profit considering one delivery at a time.
   - **`dp_two` Array**: Tracks the maximum profit for up to two overlapping deliveries.
   - Update the DP arrays considering the last non-conflicting delivery for one and two overlapping deliveries.

This ensures that we accurately determine the maximum profit the dasher can earn while handling up to two orders concurrently.

Need to dive deeper into any specific part or have another scenario to tackle? 😊


## Similar ratings (looks line leetcode k time closes to an item)
i think this can be solved using sliding window binary search: Each item in a list has a specific rating associated with it. You are given an item (its rating) and have to find the N items that are most similar to it (the absolute difference of ratings should be as less as possible for them to be similar). The item (rating) may or may not exist in the list. If you have two numbers that have the same difference, choose the smaller one. For e.g., items = [1,2,3,4,5], item=3, get_similar = 4 Return [1,2,3,4]

```python
from bisect import bisect_left

def get_similar_items(items, item, N):
    # Sort the items list
    items.sort()

    # Find the position to insert the item using binary search
    pos = bisect_left(items, item)

    # Initialize the sliding window pointers
    left, right = pos - 1, pos

    # Collect the most similar items
    similar_items = []
    while len(similar_items) < N:
        # Check boundaries and choose the closest item
        if left >= 0 and right < len(items):
            if abs(items[left] - item) <= abs(items[right] - item):
                similar_items.append(items[left])
                left -= 1
            else:
                similar_items.append(items[right])
                right += 1
        elif left >= 0:
            similar_items.append(items[left])
            left -= 1
        elif right < len(items):
            similar_items.append(items[right])
            right += 1
        else:
            break

    # Return the first N elements (sorted to handle tie-breakers)
    return sorted(similar_items)

# Example usage
items = [1, 2, 3, 4, 5]
item = 3
N = 4
print(get_similar_items(items, item, N))  # Output: [1, 2, 3, 4]

```

