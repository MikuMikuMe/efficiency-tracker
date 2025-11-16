# efficiency-tracker

Below is a basic example of an `efficiency-tracker` Python program. This program is designed to monitor and optimize employee productivity through task scheduling and resource allocation. It features error handling and comments to aid understanding.

```python
import random
import datetime
from typing import List, Dict, Any

# Sample data structure for employees and tasks
employees = [
    {"id": 1, "name": "Alice", "current_tasks": []},
    {"id": 2, "name": "Bob", "current_tasks": []},
    {"id": 3, "name": "Charlie", "current_tasks": []}
]

tasks = [
    {"id": 1, "name": "Task A", "duration": 4, "effort": 5},
    {"id": 2, "name": "Task B", "duration": 2, "effort": 3},
    {"id": 3, "name": "Task C", "duration": 3, "effort": 4},
    {"id": 4, "name": "Task D", "duration": 1, "effort": 2}
]

# Function to allocate tasks to employees intelligently
def allocate_tasks(employees: List[Dict[str, Any]], tasks: List[Dict[str, Any]]) -> None:
    if not employees or not tasks:
        raise ValueError("Employee list or task list is empty")
    
    try:
        # Sort tasks by effort (descending) for better resource allocation
        sorted_tasks = sorted(tasks, key=lambda x: x["effort"], reverse=True)
        
        for task in sorted_tasks:
            # Assign task to the employee with the least number of current tasks
            employee = min(employees, key=lambda x: len(x["current_tasks"]))
            employee["current_tasks"].append(task)
            
        print("Tasks successfully allocated")
        
    except Exception as e:
        print(f"An error occurred during task allocation: {e}")

# Function to calculate efficiency of employees
def calculate_efficiency(employees: List[Dict[str, Any]]) -> None:
    try:
        for employee in employees:
            total_time = sum(task["duration"] for task in employee["current_tasks"])
            total_effort = sum(task["effort"] for task in employee["current_tasks"])
            
            # Simple metric for efficiency: effort per time
            efficiency = total_effort / total_time if total_time > 0 else 0
            print(f"Employee {employee['name']} has an efficiency of {efficiency:.2f}.")
    
    except ZeroDivisionError:
        print("No tasks allocated, cannot calculate efficiency.")

    except Exception as e:
        print(f"An error occurred during efficiency calculation: {e}")

# Function to simulate real-time tracking
def track_efficiency() -> None:
    try:
        while True:
            print("\n--- Efficiency Tracking Report ---")
            calculate_efficiency(employees)
            print("----------------------------------")
            
            # Simulate task completion and new task generation
            for employee in employees:
                if employee["current_tasks"]:
                    employee["current_tasks"].pop(0)  # Removing completed task
            
            new_tasks = generate_random_tasks()
            allocate_tasks(employees, new_tasks)

            # Simulate a real-time delay (e.g., a day has passed)
            delay = datetime.timedelta(seconds=10) # change delay as needed for simulation
            print(f"Waiting for {delay.total_seconds()} seconds before next tracking cycle...")
            break  # For simplification, we break after one cycle. Remove break for continuous tracking

    except KeyboardInterrupt:
        print("Stopping real-time efficiency tracking.")

# Function to generate random tasks (Simulating new tasks)
def generate_random_tasks() -> List[Dict[str, Any]]:
    new_tasks = []
    task_count = random.randint(1, 3)  # Generate between 1 and 3 tasks
    for i in range(task_count):
        task_id = random.randint(5, 100)
        task = {
            "id": task_id,
            "name": f"Task {task_id}",
            "duration": random.randint(1, 5),
            "effort": random.randint(2, 6)
        }
        new_tasks.append(task)
    return new_tasks

if __name__ == "__main__":
    try:
        allocate_tasks(employees, tasks)
        track_efficiency()
    except Exception as e:
        print(f"An error occurred in the main program: {e}")
```

### Overview of the Code
- **Data Structures:** The program uses simple data structures (lists of dictionaries) for employees and tasks.
- **Task Allocation:** Tasks are allocated based on a simple heuristic (e.g., the employee with the fewest current tasks).
- **Efficiency Calculation:** Efficiency is calculated based on the sum of the task efforts divided by task times.
- **Real-time Simulation:** The `track_efficiency` function mimics a real-time monitoring system and can be adjusted to simulate different interval delays using the `datetime.timedelta` method.
- **Error Handling:** Includes error handling for empty data inputs, division-by-zero errors, and general exceptions.
- **Comments:** Code is well-commented to explain each function and key step.

This code is a simple simulation and would need further development for a production environment, such as integrating with a database, using better scheduling algorithms, and proper user interfaces for input and interaction.