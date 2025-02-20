## Buy and Shell stock problems set
The "Best Time to Buy and Sell Stock" problem is a popular problem in coding interviews and on platforms like LeetCode. There are multiple variations of the problem, each with different constraints and requirements. Below, I'll summarize the main versions of the problem and provide solutions in Python.

---

### **1. Best Time to Buy and Sell Stock (LeetCode #121)**

**Problem:**
You are given an array `prices` where `prices[i]` is the price of a given stock on the `i-th` day. You want to maximize your profit by choosing a single day to buy one stock and choosing a different day in the future to sell that stock.

**Objective:**
Return the maximum profit you can achieve. If no profit is possible, return 0.

**Solution:**

The approach is to track the minimum price seen so far and calculate the potential profit on each day by selling at the current price.

```python
def maxProfit(prices):
    min_price = float('inf')
    max_profit = 0
    
    for price in prices:
        min_price = min(min_price, price)  # Update the minimum price
        max_profit = max(max_profit, price - min_price)  # Calculate the profit
        
    return max_profit
```

**Time Complexity:** O(n), where n is the number of days (prices).
**Space Complexity:** O(1).

---

### **2. Best Time to Buy and Sell Stock II (LeetCode #122)**

**Problem:**
You are given an array `prices` where `prices[i]` is the price of a given stock on the `i-th` day. You can buy and sell the stock multiple times. For example, you may buy on one day and sell on another day, but you can never hold more than one stock at a time.

**Objective:**
Return the maximum profit you can achieve. 

**Solution:**

In this case, you should take advantage of all price increases by "buying" at the lower price and "selling" at the higher price, whenever the price is higher than the previous day's price.

```python
def maxProfit(prices):
    profit = 0
    
    for i in range(1, len(prices)):
        if prices[i] > prices[i - 1]:  # If the price is higher than previous day
            profit += prices[i] - prices[i - 1]  # Buy at previous day and sell at current day
            
    return profit
```

**Time Complexity:** O(n), where n is the number of days.
**Space Complexity:** O(1).

---

### **3. Best Time to Buy and Sell Stock III (LeetCode #123)**

**Problem:**
You are given an array `prices` where `prices[i]` is the price of a given stock on the `i-th` day. You may complete at most two transactions. You need to find the maximum profit you can achieve.

**Objective:**
Return the maximum profit you can achieve from at most two transactions.

**Solution:**

This problem can be solved using dynamic programming. The idea is to maintain two states: one for the maximum profit you can have up to the `i`-th day with one transaction, and another for two transactions.

```python
def maxProfit(prices):
    if not prices:
        return 0
    
    n = len(prices)
    
    # dp[i][0] -> max profit on day i with 0 transactions
    # dp[i][1] -> max profit on day i with 1 transaction
    # dp[i][2] -> max profit on day i with 2 transactions
    dp = [[0] * 3 for _ in range(n)]
    
    # Variables to track the max profit with a single buy/sell
    max_diff_1 = -prices[0]  # max profit after 1st buy
    max_diff_2 = -prices[0]  # max profit after 2nd buy
    
    for i in range(1, n):
        dp[i][0] = max(dp[i-1][0], dp[i-1][1])  # no profit at all
        dp[i][1] = max(dp[i-1][1], prices[i] + max_diff_1)  # profit with 1 transaction
        dp[i][2] = max(dp[i-1][2], prices[i] + max_diff_2)  # profit with 2 transactions
        
        max_diff_1 = max(max_diff_1, -prices[i])  # max of not buying or buying on i-th day
        max_diff_2 = max(max_diff_2, dp[i-1][1] - prices[i])  # max profit after second buy
        
    return dp[n-1][2]  # Max profit after 2 transactions
```

**Time Complexity:** O(n), where n is the number of days.
**Space Complexity:** O(n).

---

### **4. Best Time to Buy and Sell Stock IV (LeetCode #188)**

**Problem:**
You are given an array `prices` where `prices[i]` is the price of a given stock on the `i-th` day. You may complete at most `k` transactions. You need to find the maximum profit.

**Objective:**
Return the maximum profit you can achieve by completing at most `k` transactions.

**Solution:**

This problem is a generalization of problem #123, and we can solve it with dynamic programming. We need to keep track of the maximum profit with `k` transactions.

```python
def maxProfit(k, prices):
    if not prices:
        return 0
    
    n = len(prices)
    
    # If k is larger than half the number of days, it's equivalent to solving the problem #122
    if k >= n // 2:
        return sum(max(prices[i+1] - prices[i], 0) for i in range(n - 1))
    
    dp = [[0] * (k + 1) for _ in range(n)]
    
    for j in range(1, k + 1):
        max_diff = -prices[0]  # for tracking the max of dp[i-1][j-1] - prices[i]
        
        for i in range(1, n):
            dp[i][j] = max(dp[i-1][j], prices[i] + max_diff)
            max_diff = max(max_diff, dp[i][j-1] - prices[i])
    
    return dp[n-1][k]
```

**Time Complexity:** O(n * k), where n is the number of days and k is the number of transactions.
**Space Complexity:** O(n * k).

---

### **5. Best Time to Buy and Sell Stock V (LeetCode #309)**

**Problem:**
You are given an array `prices` where `prices[i]` is the price of a given stock on the `i-th` day. You may complete at most one transaction, but there is a cool-down period of 1 day after each sale.

**Objective:**
Return the maximum profit you can achieve with the cool-down constraint.

**Solution:**

We can use dynamic programming to solve this problem by considering the state of the stock on each day (whether we hold a stock, have just sold, or are in a cool-down period).

```python
def maxProfit(prices):
    if not prices:
        return 0
    
    n = len(prices)
    
    # Initialize the dp array with three states: 
    # dp[i][0] = max profit on day i if we do not hold stock
    # dp[i][1] = max profit on day i if we hold stock
    # dp[i][2] = max profit on day i if we are in cool down
    
    dp = [[0] * 3 for _ in range(n)]
    
    # Initial state
    dp[0][0] = 0
    dp[0][1] = -prices[0]
    dp[0][2] = 0
    
    for i in range(1, n):
        dp[i][0] = max(dp[i-1][0], dp[i-1][2])  # max of no action or cool-down state
        dp[i][1] = max(dp[i-1][1], dp[i-1][0] - prices[i])  # Buy on day i
        dp[i][2] = dp[i-1][1] + prices[i]  # Sell on day i, transition from holding state
    
    return max(dp[n-1][0], dp[n-1][2])  # Return the max profit after final day
```

**Time Complexity:** O(n), where n is the number of days.
**Space Complexity:** O(n).

---

### **Conclusion:**
These solutions use dynamic programming or greedy approaches to maximize profit for different stock-buying strategies, depending on the constraints of each variation. For all of these problems, understanding the relationship between different states (e.g., holding stock, selling, or cool-down) is key to solving them optimally.
Yes, there is another variation of the "Best Time to Buy and Sell Stock" problem on LeetCode that includes a **transaction fee**. This problem is **LeetCode #714: Best Time to Buy and 
Sell Stock with Transaction Fee**.

### **LeetCode #714: Best Time to Buy and Sell Stock with Transaction Fee**

**Problem:**
You are given an array `prices` where `prices[i]` is the price of a given stock on the `i-th` day. You are also given an integer `fee`, which is the transaction fee you must pay whenever you complete a buy/sell transaction.

**Objective:**
Return the maximum profit you can achieve by completing as many transactions as you like, where the transaction fee is applied after each sell.

### **Solution:**

This is another variation where you can apply dynamic programming, similar to the previous problems, but the key difference is that we need to account for the transaction fee when we sell a stock. We must adjust our decision-making to take the fee into consideration.

We can define two states:
1. `hold[i]` - The maximum profit we can have on day `i` if we **hold** a stock.
2. `cash[i]` - The maximum profit we can have on day `i` if we **do not hold** a stock.

At each step, we either:
- Buy a stock (transition from cash to hold), or
- Sell a stock (transition from hold to cash), but we must subtract the fee from the selling price.

### **Dynamic Programming Approach:**

1. `hold[i]` can either:
   - Be the same as `hold[i-1]` (meaning we didn't buy today).
   - Or we can transition from `cash[i-1]` by buying at the current price: `hold[i] = max(hold[i-1], cash[i-1] - prices[i])`.

2. `cash[i]` can either:
   - Be the same as `cash[i-1]` (meaning we didn't sell today).
   - Or we can transition from `hold[i-1]` by selling the stock at today's price and subtracting the transaction fee: `cash[i] = max(cash[i-1], hold[i-1] + prices[i] - fee)`.

At the end of the array, the result will be the value in `cash[n-1]`, because we want the maximum profit without holding any stock.

### **Code Implementation:**

```python
def maxProfit(prices, fee):
    n = len(prices)
    
    # Initialize the variables for holding stock and not holding stock.
    hold = -prices[0]  # If we buy the stock on the first day
    cash = 0  # If we don't buy the stock on the first day
    
    for i in range(1, n):
        # Update the state where we hold the stock
        hold = max(hold, cash - prices[i])
        
        # Update the state where we don't hold the stock (cash state)
        cash = max(cash, hold + prices[i] - fee)
    
    return cash
```

### **Explanation:**

1. **Initialization:**
   - `hold = -prices[0]`: Initially, we buy the stock on day 0, so our holding state is the negative price of the stock on the first day.
   - `cash = 0`: Initially, we don't hold any stock, so the cash state is 0 profit.

2. **Iterate over the prices:**
   - For each day `i` (starting from day 1), update the `hold` and `cash` states:
     - `hold = max(hold, cash - prices[i])`: The best profit we can have by holding a stock is either the same as before (`hold`) or the profit from having sold a stock the previous day and buying it today.
     - `cash = max(cash, hold + prices[i] - fee)`: The best profit we can have by selling a stock today is either the same as before (`cash`) or the profit from holding the stock and selling it today after accounting for the transaction fee.

3. **Return the result:**
   - After iterating through all the days, the maximum profit we can have without holding any stock at the end will be stored in `cash`.

### **Time Complexity:**
- **O(n)** where `n` is the number of days (i.e., the length of the `prices` array). This is because we are only iterating through the list once.

### **Space Complexity:**
- **O(1)** because we are only using a constant amount of space (i.e., two variables `hold` and `cash`) to store the states.

---

### **Example:**

Let's walk through an example to better understand the solution:

```python
prices = [1, 3, 2, 8, 4, 9]
fee = 2
print(maxProfit(prices, fee))
```

#### Step-by-step breakdown:
- Day 0: `prices[0] = 1`
  - `hold = -1` (bought stock)
  - `cash = 0` (no transaction yet)
  
- Day 1: `prices[1] = 3`
  - `hold = max(-1, 0 - 3) = -1` (no change in hold state)
  - `cash = max(0, -1 + 3 - 2) = 0` (no transaction, no profit)

- Day 2: `prices[2] = 2`
  - `hold = max(-1, 0 - 2) = -1` (no change in hold state)
  - `cash = max(0, -1 + 2 - 2) = 0` (no transaction, no profit)

- Day 3: `prices[3] = 8`
  - `hold = max(-1, 0 - 8) = -1` (no change in hold state)
  - `cash = max(0, -1 + 8 - 2) = 5` (sell on day 3: profit = 8 - 1 - 2 = 5)

- Day 4: `prices[4] = 4`
  - `hold = max(-1, 5 - 4) = 1` (buy on day 4: profit = 5 - 4 = 1)
  - `cash = max(5, 1 + 4 - 2) = 5` (no change in cash state)

- Day 5: `prices[5] = 9`
  - `hold = max(1, 5 - 9) = 1` (no change in hold state)
  - `cash = max(5, 1 + 9 - 2) = 8` (sell on day 5: profit = 9 - 4 - 2 = 8)

The final profit is `cash = 8`, which is the maximum profit achievable with a transaction fee of 2.

---

### Conclusion:
The **Best Time to Buy and Sell Stock with Transaction Fee** problem can be efficiently solved using dynamic programming. The key difference from previous problems is that the transaction fee must be taken into account when calculating the profit from selling. This solution runs in linear time and uses constant space, making it both time-efficient and space-efficient.
