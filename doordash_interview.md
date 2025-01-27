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
