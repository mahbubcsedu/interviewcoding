### find min steps for knight to move from (0,0) to (x,y) when board in infinite
To determine the minimum number of moves for a knight to travel from position (0, 0) to (x, y) on an infinite plane, we can use a Breadth-First Search (BFS) algorithm. This approach ensures that we find the shortest path in terms of moves.

Here's the result for moving a knight from (0, 0) to (5, 5):

**Minimum number of moves to reach (5, 5): 4**

### Explanation:
1. **Possible Moves**: A knight in chess can move in an "L" shape: two squares in one direction and one square perpendicular, or one square in one direction and two squares perpendicular.
2. **BFS Algorithm**: We use BFS to explore all possible moves level by level, ensuring we find the shortest path.

### Example Code:
```python
from collections import deque

def min_knight_moves(x, y):
    # Possible moves for a knight in chess
    moves = [(2, 1), (1, 2), (-1, 2), (-2, 1), 
             (-2, -1), (-1, -2), (1, -2), (2, -1)]
    
    # Use BFS to find the shortest path
    queue = deque([(0, 0, 0)])  # (current_x, current_y, steps)
    visited = set((0, 0))
    
    while queue:
        cur_x, cur_y, steps = queue.popleft()
        
        if cur_x == x and cur_y == y:
            return steps
        
        for move in moves:
            next_x, next_y = cur_x + move[0], cur_y + move[1]
            
            if (next_x, next_y) not in visited:
                visited.add((next_x, next_y))
                queue.append((next_x, next_y, steps + 1))

# Example usage
x, y = 5, 5
print(f"Minimum number of moves to reach ({x}, {y}): {min_knight_moves(x, y)}")
```

Feel free to try different coordinates or ask if you have any further questions!
