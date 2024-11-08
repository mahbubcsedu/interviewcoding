## In a organization we are trying to fine whether manager has salary to the average of its reports. 
To determine if a manager has an average salary equal to the average salary of their direct reports, we can break down the problem into steps. Here’s a plan for the algorithm:

1. **Input Structure**:
   - You’ll need a list of employees, each with an ID, manager ID, and salary.
   - Each employee has a unique ID and each manager ID points to another employee ID (representing the manager).

2. **Constructing the Relationships**:
   - Use a dictionary to map each manager to their direct reports.
   - For each employee, add them to the list of reports of their respective manager.

3. **Calculate Salaries**:
   - For each manager, calculate the average salary of their direct reports.
   - Check if this average equals the manager’s own salary.

### Algorithm Steps

1. **Build a `reports` dictionary** that maps each manager’s ID to a list of IDs of their direct reports.
2. **Traverse each manager** in the `reports` dictionary:
   - If the manager has any reports:
     - Calculate the average salary of the reports.
     - Compare it to the manager’s salary.
   - If they match, record the manager’s ID as having the desired property.

### Code Implementation

Here’s a Python solution for the problem:

```python
from collections import defaultdict

def find_managers_with_matching_average_salary(employees):
    # Dictionary to store employee salaries by ID
    employee_salaries = {emp['id']: emp['salary'] for emp in employees}
    # Dictionary to store direct reports by manager ID
    reports = defaultdict(list)
    
    # Fill the reports dictionary
    for emp in employees:
        if emp['manager_id'] is not None:  # If the employee has a manager
            reports[emp['manager_id']].append(emp['id'])
    
    # List to hold managers with average salary equal to their salary
    matching_managers = []
    
    # Check each manager and their reports
    for manager_id, report_ids in reports.items():
        if report_ids:  # Ensure the manager has direct reports
            # Calculate the average salary of the reports
            total_salary = sum(employee_salaries[emp_id] for emp_id in report_ids)
            avg_salary = total_salary / len(report_ids)
            
            # Compare manager's salary to the average salary of their reports
            if employee_salaries[manager_id] == avg_salary:
                matching_managers.append(manager_id)
    
    return matching_managers

# Example usage
employees = [
    {'id': 1, 'salary': 100, 'manager_id': None},  # CEO, no manager
    {'id': 2, 'salary': 80, 'manager_id': 1},
    {'id': 3, 'salary': 120, 'manager_id': 1},
    {'id': 4, 'salary': 90, 'manager_id': 2},
    {'id': 5, 'salary': 70, 'manager_id': 2},
]

print("Managers with matching average salary:", find_managers_with_matching_average_salary(employees))
```

### Explanation

1. **Data Setup**: `employee_salaries` dictionary stores the salary of each employee by their ID for quick lookup.
2. **Building the Reports Dictionary**: We populate `reports` such that each manager ID maps to a list of their direct reports' IDs.
3. **Calculating the Average Salary for Each Manager**:
   - For each manager, we calculate the average salary of their direct reports by summing the salaries of all direct reports and dividing by the number of reports.
4. **Comparison**: If a manager’s salary matches the calculated average, their ID is added to `matching_managers`.
5. **Output**: The function returns a list of manager IDs where each manager's salary matches the average salary of their direct reports.

### Complexity Analysis

- **Time Complexity**: \(O(n)\), where \(n\) is the number of employees, since we iterate over employees to populate the data structures and calculate averages.
- **Space Complexity**: \(O(n)\), due to the additional dictionaries for storing salaries and relationships.

This algorithm efficiently finds managers with an average salary equal to their reports' average salary.

## Median instead of average
To find if a manager’s salary is equal to the **median salary** of their direct reports instead of the average, the algorithm changes slightly:

1. Instead of calculating the average, we need to calculate the **median salary** of the direct reports.
2. The median requires sorting the list of direct reports' salaries and then selecting the middle value (or the average of the two middle values if the number of reports is even).

### Updated Algorithm Steps

1. **Construct the Reports Dictionary**: Same as before.
2. **Calculate the Median Salary**:
   - For each manager, collect the salaries of their direct reports.
   - Sort this list of salaries.
   - Calculate the median:
     - If the number of reports is odd, the median is the middle element.
     - If the number of reports is even, the median is the average of the two middle elements.
3. **Comparison**: Check if the manager’s salary is equal to this median value.

### Code Implementation

Here’s the Python code for calculating the median instead of the average:

```python
from collections import defaultdict

def find_managers_with_matching_median_salary(employees):
    # Dictionary to store employee salaries by ID
    employee_salaries = {emp['id']: emp['salary'] for emp in employees}
    # Dictionary to store direct reports by manager ID
    reports = defaultdict(list)
    
    # Fill the reports dictionary
    for emp in employees:
        if emp['manager_id'] is not None:  # If the employee has a manager
            reports[emp['manager_id']].append(emp['id'])
    
    # List to hold managers with median salary equal to their salary
    matching_managers = []
    
    # Check each manager and their reports
    for manager_id, report_ids in reports.items():
        if report_ids:  # Ensure the manager has direct reports
            # Collect the salaries of all reports
            report_salaries = sorted(employee_salaries[emp_id] for emp_id in report_ids)
            
            # Calculate the median salary of the reports
            num_reports = len(report_salaries)
            if num_reports % 2 == 1:
                # Odd number of reports, median is the middle element
                median_salary = report_salaries[num_reports // 2]
            else:
                # Even number of reports, median is the average of the two middle elements
                median_salary = (report_salaries[num_reports // 2 - 1] + report_salaries[num_reports // 2]) / 2
            
            # Compare manager's salary to the median salary of their reports
            if employee_salaries[manager_id] == median_salary:
                matching_managers.append(manager_id)
    
    return matching_managers

# Example usage
employees = [
    {'id': 1, 'salary': 100, 'manager_id': None},  # CEO, no manager
    {'id': 2, 'salary': 80, 'manager_id': 1},
    {'id': 3, 'salary': 120, 'manager_id': 1},
    {'id': 4, 'salary': 90, 'manager_id': 2},
    {'id': 5, 'salary': 70, 'manager_id': 2},
]

print("Managers with matching median salary:", find_managers_with_matching_median_salary(employees))
```

### Explanation of Median Calculation

1. **Sorting Salaries**: For each manager, sort the list of salaries of their direct reports.
2. **Finding the Median**:
   - If the number of reports is odd, select the middle element in the sorted list.
   - If the number of reports is even, calculate the median as the average of the two middle elements.
3. **Comparison**: Check if the manager’s salary is equal to this calculated median salary.

### Complexity Analysis

- **Time Complexity**: Sorting the salaries for each manager takes \(O(k \log k)\), where \(k\) is the number of reports per manager. In the worst case, if every employee is a report of a single manager, this could be \(O(n \log n)\). If each manager has only a few reports, this becomes close to \(O(n)\).
- **Space Complexity**: \(O(n)\), as we store the relationships and salaries.

This code efficiently finds managers whose salary equals the median salary of their direct reports.

## Optimized median
To efficiently find the median using a **two-heap** (or two-queue) approach, we can use a **max-heap** to store the lower half of the salaries and a **min-heap** to store the upper half. This approach allows us to keep track of the median in \(O(\log k)\) time for each insertion, where \(k\) is the number of reports.

### Explanation of the Two-Heap Approach

1. **Two Heaps**:
   - **Max-Heap (lower half)**: Keeps track of the lower half of the salaries. The maximum value in this heap is the largest value in the lower half.
   - **Min-Heap (upper half)**: Keeps track of the upper half of the salaries. The minimum value in this heap is the smallest value in the upper half.

2. **Balancing the Heaps**:
   - Each time a new salary is added, we decide which heap it should go into based on its value compared to the tops of the heaps.
   - We keep the heaps balanced in size (or at most one element difference), so that we can easily retrieve the median:
     - If both heaps are the same size, the median is the average of the tops of both heaps.
     - If one heap has one more element, the median is the top of that heap.

3. **Adding and Balancing**:
   - Insert the salary into the appropriate heap.
   - Rebalance the heaps if necessary to maintain the size property.

### Code Implementation

Here’s how you can implement this algorithm in Python to determine if a manager’s salary matches the median salary of their direct reports.

```python
import heapq
from collections import defaultdict

class MedianFinder:
    def __init__(self):
        self.low = []  # Max-heap for the lower half
        self.high = []  # Min-heap for the upper half
    
    def add_num(self, num):
        # Add a number to the heaps
        if not self.low or num <= -self.low[0]:
            heapq.heappush(self.low, -num)  # Use negative to simulate max-heap in Python
        else:
            heapq.heappush(self.high, num)
        
        # Balance the heaps
        if len(self.low) > len(self.high) + 1:
            heapq.heappush(self.high, -heapq.heappop(self.low))
        elif len(self.high) > len(self.low):
            heapq.heappush(self.low, -heapq.heappop(self.high))
    
    def find_median(self):
        # Find the median
        if len(self.low) > len(self.high):
            return -self.low[0]
        return (-self.low[0] + self.high[0]) / 2

def find_managers_with_matching_median_salary(employees):
    # Dictionary to store employee salaries by ID
    employee_salaries = {emp['id']: emp['salary'] for emp in employees}
    # Dictionary to store direct reports by manager ID
    reports = defaultdict(list)
    
    # Fill the reports dictionary
    for emp in employees:
        if emp['manager_id'] is not None:  # If the employee has a manager
            reports[emp['manager_id']].append(emp['id'])
    
    # List to hold managers with median salary equal to their salary
    matching_managers = []
    
    # Check each manager and their reports
    for manager_id, report_ids in reports.items():
        if report_ids:  # Ensure the manager has direct reports
            # Initialize a MedianFinder instance
            median_finder = MedianFinder()
            
            # Add each report's salary to the median finder
            for emp_id in report_ids:
                median_finder.add_num(employee_salaries[emp_id])
            
            # Calculate the median salary of the reports
            median_salary = median_finder.find_median()
            
            # Compare manager's salary to the median salary of their reports
            if employee_salaries[manager_id] == median_salary:
                matching_managers.append(manager_id)
    
    return matching_managers

# Example usage
employees = [
    {'id': 1, 'salary': 100, 'manager_id': None},  # CEO, no manager
    {'id': 2, 'salary': 80, 'manager_id': 1},
    {'id': 3, 'salary': 120, 'manager_id': 1},
    {'id': 4, 'salary': 90, 'manager_id': 2},
    {'id': 5, 'salary': 70, 'manager_id': 2},
]

print("Managers with matching median salary:", find_managers_with_matching_median_salary(employees))
```

### Explanation of the Code

1. **`MedianFinder` Class**:
   - Maintains two heaps, `low` (max-heap for the lower half) and `high` (min-heap for the upper half).
   - **`add_num` method**: Adds a salary to the appropriate heap and rebalances if needed.
   - **`find_median` method**: Computes the median based on the heaps' sizes.

2. **`find_managers_with_matching_median_salary` function**:
   - Builds the `reports` dictionary, then iterates over each manager.
   - For each manager with reports:
     - Initializes a `MedianFinder` instance to dynamically calculate the median of the report salaries.
     - Compares the calculated median to the manager’s salary.

### Complexity Analysis

- **Time Complexity**: Each insertion in `MedianFinder` is \(O(\log k)\), where \(k\) is the number of reports. Thus, for \(m\) managers and \(k\) reports per manager, the complexity is approximately \(O(m \cdot k \log k)\).
- **Space Complexity**: \(O(k)\) per manager, due to the two heaps storing salaries.

This two-heap solution efficiently maintains a median as reports are added, and avoids the need to repeatedly sort lists of salaries, making it optimal for large inputs.
