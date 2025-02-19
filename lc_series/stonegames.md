# Stone Game Series Wiki

## 1. 877. Stone Game

**Problem Statement**: 
Alice and Bob play a game with piles of stones. There are an even number of piles arranged in a row, and each pile has a positive integer number of stones piles[i]. The objective is to determine if Alice wins, assuming both players play optimally.

**Solution**:
```python
def stoneGame(piles):
    return True
```

---

## 2. 1140. Stone Game II

**Problem Statement**: 
Alice and Bob play a game with piles of stones, taking turns to take all the stones in the first X remaining piles, where 1 <= X <= 2M. M is updated to be the maximum of M and X. The objective is to find the maximum number of stones Alice can get.

**Solution**:
```python
def stoneGameII(piles):
    n = len(piles)
    dp = [[0] * (n + 1) for _ in range(n + 1)]
    suffix_sum = [0] * (n + 1)
    
    for i in range(n - 1, -1, -1):
        suffix_sum[i] = suffix_sum[i + 1] + piles[i]
    
    def dfs(i, M):
        if i == n:
            return 0
        if dp[i][M] > 0:
            return dp[i][M]
        max_stones = 0
        for x in range(1, 2 * M + 1):
            if i + x <= n:
                max_stones = max(max_stones, suffix_sum[i] - dfs(i + x, max(M, x)))
        dp[i][M] = max_stones
        return max_stones

    return dfs(0, 1)
```

---

## 3. 1406. Stone Game III

**Problem Statement**: 
Alice and Bob continue their games with piles of stones, but this time, the game allows them to pick stones from either end of the row of piles. The objective is to determine if Alice wins, loses, or if the game ends in a draw, assuming both players play optimally.

**Solution**:
```python
def stoneGameIII(piles):
    n = len(piles)
    dp = [-float('inf')] * (n + 1)
    dp[n] = 0
    for i in range(n - 1, -1, -1):
        s = 0
        for k in range(1, 4):
            if i + k <= n:
                s += piles[i + k - 1]
                dp[i] = max(dp[i], s - dp[i + k])
    return "Alice" if dp[0] > 0 else "Bob" if dp[0] < 0 else "Tie"
```

---

## 4. 1510. Stone Game IV

**Problem Statement**: 
Alice and Bob play a game with piles of stones, where in each turn, a player can take a square number of stones. The objective is to determine if Alice wins, given that both players play optimally.

**Solution**:
```python
def winnerSquareGame(n):
    dp = [False] * (n + 1)
    for i in range(1, n + 1):
        j = 1
        while j * j <= i:
            if not dp[i - j * j]:
                dp[i] = True
                break
            j += 1
    return dp[n]
```

---

## 5. 1563. Stone Game V

**Problem Statement**: 
Alice and Bob play a game with piles of stones arranged in a row, where they take turns to pick stones from any one of the first K remaining piles. The objective is to find the maximum score Alice can achieve, assuming both players play optimally.

**Solution**:
```python
def stoneGameV(piles):
    n = len(piles)
    prefix_sum = [0] * (n + 1)
    for i in range(n):
        prefix_sum[i + 1] = prefix_sum[i] + piles[i]
    
    dp = [[0] * n for _ in range(n)]
    for length in range(1, n + 1):
        for i in range(n - length + 1):
            j = i + length - 1
            for k in range(i, j):
                left = prefix_sum[k + 1] - prefix_sum[i]
                right = prefix_sum[j + 1] - prefix_sum[k + 1]
                if left <= right:
                    dp[i][j] = max(dp[i][j], left + (dp[i][k] if i < k else 0))
                if right <= left:
                    dp[i][j] = max(dp[i][j], right + (dp[k + 1][j] if k + 1 < j else 0))
    return dp[0][n - 1]
```

---

## 6. 1686. Stone Game VI

**Problem Statement**: 
Alice and Bob play a game with piles of stones, where each player can take stones from either end of the row. The objective is to determine who wins, assuming both players play optimally.

**Solution**:
```python
def stoneGameVI(piles):
    n = len(piles)
    dp = [-float('inf')] * (n + 1)
    dp[n] = 0
    for i in range(n - 1, -1, -1):
        s = 0
        for k in range(1, 4):
            if i + k <= n:
                s += piles[i + k - 1]
                dp[i] = max(dp[i], s - dp[i + k])
    return "Alice" if dp[0] > 0 else "Bob" if dp[0] < 0 else "Tie"
```

---

## 7. 1690. Stone Game VII

**Problem Statement**: 
Alice and Bob play a game with piles of stones, where they take turns to pick stones from the ends of the row. The objective is to find the maximum score difference Alice can achieve over Bob, assuming both players play optimally.

**Solution**:
```python
def stoneGameVII(piles):
    n = len(piles)
    total_sum = sum(piles)
    dp = [[0] * n for _ in range(n)]
    for length in range(2, n + 1):
        for i in range(n - length + 1):
            j = i + length - 1
            dp[i][j] = max(total_sum - piles[i] - dp[i + 1][j], total_sum - piles[j] - dp[i][j - 1])
            total_sum -= piles[i]
    return dp[0][n - 1]
```

---

## 8. 1872. Stone Game VIII

**Problem Statement**: 
Alice and Bob play a game with piles of stones, where they can pick stones from either end of the row. The objective is to determine the maximum score Alice can achieve, assuming both players play optimally.

**Solution**:
```python
def stoneGameVIII(piles):
    n = len(piles)
    suffix_sum = [0] * (n + 1)
    for i in range(n - 1, -1, -1):
        suffix_sum[i] = suffix_sum[i + 1] + piles[i]
    
    dp = [-float('inf')] * n
    dp[n - 1] = suffix_sum[n - 1]
    
    for i in range(n - 2, 0, -1):
        dp[i] = max(dp[i + 1], suffix_sum[i] - dp[i + 1])
    
    return dp[1]
```

---

## 9. 2029. Stone Game IX

**Problem Statement**: 
Alice and Bob play a game with piles of stones, where they take turns to pick stones. The objective is to determine who wins, assuming both players play optimally.

**Solution**:
```python
def stoneGameIX(piles):
    count = [0] * 3
    for pile in piles:
        count[pile % 3] += 1
    
    if count[0] % 2 == 0:
        return count[1] > 0 and count[2] > 0
    return abs(count[1] - count[2]) > 2
```

