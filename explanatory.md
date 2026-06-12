# Beginner Explanatory Guide: OPS-402: Fix Broken Alerting Rules

> **Task Type**: Service Task  
> **Domain/Focus**: Python Fundamentals, Alerting Systems

---

## 1. The Goal (In-Depth Beginner Explanation)

### The Core Problem
The alerting system in our application is currently malfunctioning, leading to significant issues in monitoring the health of our services. Specifically, it is firing critical alerts for minor issues while failing to alert on actual problems. This misbehavior stems from three broken alert rules that have incorrect conditions. For instance, the CPU alert is set to trigger at a threshold of greater than 9%, which is far too low to indicate a real problem. Similarly, the memory alert is incorrectly checking the free memory percentage instead of the used percentage, which can lead to missed alerts when memory usage is high. Lastly, the error rate alert is based on total error counts rather than the per-minute rate, which can misrepresent the actual error situation.

Fixing these alert rules is crucial for maintaining the reliability of our system. If alerts are not firing correctly, it can lead to a lack of awareness about critical issues, resulting in potential downtime or degraded performance. Users depend on accurate alerts to ensure that the system is functioning optimally, and any failure in this area can lead to a loss of trust and increased operational costs.

### Jargon Buster (Key Terms Explained)
* **Alerting System**: An alerting system is a mechanism that monitors the performance and health of applications or services. It sends notifications (alerts) when certain predefined conditions are met, such as high CPU usage or memory consumption. For example, if a server's CPU usage exceeds 90%, the alerting system should notify the operations team to investigate.

* **Threshold**: A threshold is a specific value that triggers an alert when crossed. For instance, if the CPU usage threshold is set at 90%, an alert will be generated whenever the CPU usage exceeds this value. This helps in identifying potential issues before they escalate.

* **Metric**: A metric is a quantifiable measure used to track the performance of a system. Common metrics include CPU usage, memory usage, and error rates. For example, CPU usage can be measured as a percentage of total capacity being used at any given time.

* **Operator**: An operator is a symbol that defines the relationship between two values in a condition. Common operators include greater than (`>`), less than (`<`), and equal to (`==`). For example, in the condition `cpu_percent > 90`, the operator `>` checks if the CPU percentage exceeds 90.

### Expected Outcome
After implementing the necessary fixes, the alerting system should behave as follows:

- **Before**: The CPU alert triggers at > 9%, leading to false alerts for minor CPU usage. The memory alert checks free memory instead of used memory, potentially missing critical alerts. The error rate alert uses total errors, which can mislead the team about the actual error situation.
  
- **After**: The CPU alert correctly triggers at > 90%, ensuring that only significant CPU usage generates alerts. The memory alert checks the used percentage, accurately reflecting memory consumption. The error rate alert uses the per-minute rate, providing a true picture of the error situation.

---

## 2. Related Coding Concepts & Syntax (50% Theory, 50% Practice)

### Concept 1: Conditional Statements
#### 📘 Theoretical Overview (50%)
Conditional statements are fundamental programming constructs that allow the execution of certain blocks of code based on whether a condition evaluates to true or false. They are essential for decision-making in code. Without conditional statements, a program would execute the same sequence of instructions regardless of the input, making it inflexible and unable to respond to different situations.

In Python, conditional statements are implemented using `if`, `elif`, and `else`. The `if` statement checks a condition, and if it is true, the code block under it executes. If it is false, the program can check additional conditions using `elif` or execute an alternative block with `else`.

#### 💻 Syntax & Practical Examples (50%)
* **Language Syntax**:
  ```python
  if condition:
      # Code to execute if condition is true
  elif another_condition:
      # Code to execute if another_condition is true
  else:
      # Code to execute if none of the above conditions are true
  ```

* **Real-World Application**:
  ```python
  cpu_usage = 95
  if cpu_usage > 90:
      print("Alert: High CPU usage!")
  elif cpu_usage > 70:
      print("Warning: CPU usage is getting high.")
  else:
      print("CPU usage is normal.")
  ```

In this example, the program checks the CPU usage and prints an alert if it exceeds 90%, a warning if it exceeds 70%, and a normal message otherwise.

---

## 3. Step-by-Step Logic & Walkthrough

1. **Step 1: Locate and Analyze the Target File**
   * Navigate to the `alertingEngine.py` file within the `s-w12-task-04` folder. This file contains the logic for the alerting rules.
   * Focus on the `create_default_rules` function, specifically the lines where the alert rules are added using the `add_rule` method.

2. **Step 2: Input Verification & Validation**
   * Check the existing rules for correctness. The CPU alert should trigger at > 90%, the memory alert should check used memory, and the error rate alert should use the per-minute rate.
   * Identify the parameters being passed to `add_rule` for each alert.

3. **Step 3: Core Implementation / Modification**
   * Modify the `add_rule` calls in the `create_default_rules` function:
     - Change the CPU alert threshold from 9 to 90.
     - Change the memory alert to check `used_memory_percent` instead of `free_memory_percent`.
     - Change the error rate alert to use `errors_per_minute` instead of `total_errors`.

4. **Step 4: Output Verification & Testing**
   * After making the changes, run the test suite using `pytest` to ensure all tests pass. This will verify that the alerting rules are functioning as expected.

---

## 4. Detailed Walkthrough of Test Cases

### Test Case 1: Standard / Success Case
* **Description**: This test checks if the alerting system correctly triggers an alert for high CPU usage.
* **Inputs**:
  ```json
  {
    "cpu_percent": 95
  }
  ```
* **Step-by-Step Execution Trace**:
  1. The `check_metrics` function receives the input values.
  2. It checks the CPU usage against the rule for "High CPU" which is set to trigger at > 90%.
  3. Since 95 > 90 is true, the alert is generated and added to the list of fired alerts.
* **Expected Output**: 
  ```json
  [
    {
      "rule": "High CPU",
      "value": 95,
      "threshold": 90,
      "severity": "critical"
    }
  ]
  ```

### Test Case 2: Edge Case / Validation Fail
* **Description**: This test checks if the alerting system does not trigger an alert for normal CPU usage.
* **Inputs**:
  ```json
  {
    "cpu_percent": 50
  }
  ```
* **Step-by-Step Execution Trace**:
  1. The `check_metrics` function receives the input values.
  2. It checks the CPU usage against the rule for "High CPU" which is set to trigger at > 90%.
  3. Since 50 > 90 is false, no alert is generated.
* **Expected Output**: 
  ```json
  []
  ``` 

This detailed guide should provide a comprehensive understanding of the task at hand, the necessary coding concepts, and the steps required to implement the solution effectively.