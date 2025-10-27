# Chapter 9: Functions - Defining and Calling Functions

## Introduction

You've mastered loops and conditionals. Now let's explore **functions** - reusable blocks of code that perform specific tasks. Functions are fundamental to organizing and structuring programs.

Common errors:
- **TypeError**: Wrong number or type of arguments
- **NameError**: Function not defined or wrong scope
- **UnboundLocalError**: Variable scope issues
- **RecursionError**: Infinite recursion
- Return value errors

Let's master functions!

---

## 9.1 Function Basics

### Defining and Calling Functions

```python
# Basic function definition
def greet():
    print("Hello!")

# Call the function
greet()  # Prints: Hello!

# Function with parameters
def greet_person(name):
    print(f"Hello, {name}!")

greet_person("Alice")  # Prints: Hello, Alice!

# Function with return value
def add(a, b):
    return a + b

result = add(5, 3)  # result = 8

# Function with multiple parameters
def calculate(x, y, operation):
    if operation == "add":
        return x + y
    elif operation == "multiply":
        return x * y

result = calculate(5, 3, "add")  # 8
```

---

### Error Type 1: `TypeError: function() takes X positional arguments but Y were given`

**Error Message:**
```python
>>> def greet(name):
...     print(f"Hello, {name}!")
>>> greet()
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: greet() missing 1 required positional argument: 'name'
```

**What Happened:**
Called function with wrong number of arguments.

**Why It Happens:**
- Missing required arguments
- Too many arguments
- Forgetting self in methods
- Wrong parameter count

**Code Example - WRONG:**
```python
# Missing argument
def greet(name):
    print(f"Hello, {name}!")

greet()  # ERROR! Missing 'name'

# Too many arguments
def add(a, b):
    return a + b

result = add(1, 2, 3)  # ERROR! Too many arguments

# Wrong number of arguments
def calculate(x, y, z):
    return x + y + z

result = calculate(1, 2)  # ERROR! Missing z

# Mixing positional and keyword wrong
def process(a, b, c):
    return a + b + c

result = process(1, c=3)  # ERROR! Missing b
```

**Code Example - CORRECT:**
```python
# Provide all required arguments
def greet(name):
    print(f"Hello, {name}!")

greet("Alice")  # ✓ Correct

# Match parameter count
def add(a, b):
    return a + b

result = add(1, 2)  # ✓ Correct

# Use default parameters for optional arguments
def greet(name, greeting="Hello"):
    print(f"{greeting}, {name}!")

greet("Alice")  # ✓ Uses default: "Hello, Alice!"
greet("Bob", "Hi")  # ✓ Custom: "Hi, Bob!"

# Use *args for variable arguments
def add_all(*numbers):
    return sum(numbers)

result = add_all(1, 2)  # ✓ Works
result = add_all(1, 2, 3, 4, 5)  # ✓ Also works

# Use **kwargs for keyword arguments
def print_info(**info):
    for key, value in info.items():
        print(f"{key}: {value}")

print_info(name="Alice", age=25)  # ✓

# Mix positional, default, *args, **kwargs
def complex_function(required, optional="default", *args, **kwargs):
    print(f"Required: {required}")
    print(f"Optional: {optional}")
    print(f"Args: {args}")
    print(f"Kwargs: {kwargs}")

complex_function("test")  # ✓
complex_function("test", "custom", 1, 2, key="value")  # ✓
```

---

## 9.2 Return Values

### Understanding Returns

```python
# Function with return
def add(a, b):
    return a + b

result = add(3, 5)  # result = 8

# Function without return (returns None)
def greet(name):
    print(f"Hello, {name}!")
    # No return statement

result = greet("Alice")  # result = None

# Multiple return values (returns tuple)
def get_stats(numbers):
    return min(numbers), max(numbers), sum(numbers)

min_val, max_val, total = get_stats([1, 2, 3, 4, 5])

# Early return
def check_age(age):
    if age < 0:
        return "Invalid"
    if age < 18:
        return "Minor"
    return "Adult"
```

---

### Error Type 2: `TypeError: 'NoneType' object is not...`

**Error Message:**
```python
>>> def add(a, b):
...     result = a + b
...     # Forgot return!
>>> total = add(3, 5)
>>> print(total + 10)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: unsupported operand type(s) for +: 'NoneType' and 'int'
```

**What Happened:**
Function returns None (no return statement), trying to use it.

**Why It Happens:**
- Forgetting return statement
- Return in wrong place
- Conditional return missing else

**Code Example - WRONG:**
```python
# Forgot return
def add(a, b):
    result = a + b
    # ERROR! No return

total = add(3, 5)  # None
print(total + 10)  # ERROR! None + 10

# Return in wrong place
def calculate(x):
    if x > 0:
        return x * 2
    # ERROR! No return for x <= 0

result = calculate(-5)  # None

# Indentation error
def multiply(a, b):
    result = a * b
return result  # ERROR! Wrong indentation

# After return is unreachable
def process():
    return "done"
    print("After")  # Never executes
```

**Code Example - CORRECT:**
```python
# Include return statement
def add(a, b):
    result = a + b
    return result  # ✓

total = add(3, 5)  # 8
print(total + 10)  # ✓ 18

# Return in all branches
def calculate(x):
    if x > 0:
        return x * 2
    else:
        return 0  # ✓ Return for all cases

# Or single return at end
def calculate(x):
    if x > 0:
        result = x * 2
    else:
        result = 0
    return result  # ✓

# Correct indentation
def multiply(a, b):
    result = a * b
    return result  # ✓ Indented

# Check for None before using
def get_value():
    return None

value = get_value()
if value is not None:  # ✓ Check first
    print(value + 10)
else:
    print("No value returned")

# Return default value
def safe_divide(a, b):
    if b == 0:
        return None
    return a / b

result = safe_divide(10, 0)  # None
if result is not None:  # ✓ Safe
    print(result)
```

---

## 9.3 Variable Scope

### Understanding Scope

```python
# Global variable
global_var = "I'm global"

def function():
    # Local variable
    local_var = "I'm local"
    print(global_var)  # ✓ Can read global
    print(local_var)   # ✓ Can read local

function()
# print(local_var)  # ERROR! local_var not accessible

# Modifying global (need 'global' keyword)
counter = 0

def increment():
    global counter  # Declare as global
    counter += 1

increment()
print(counter)  # 1
```

---

### Error Type 3: `UnboundLocalError: local variable 'X' referenced before assignment`

**Error Message:**
```python
>>> count = 0
>>> def increment():
...     count = count + 1
>>> increment()
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "<stdin>", line 2, in increment
UnboundLocalError: local variable 'count' referenced before assignment
```

**What Happened:**
Trying to modify global variable without declaring it as global.

**Why It Happens:**
- Modifying global without 'global' keyword
- Variable shadowing
- Accessing before assignment in same scope

**Code Example - WRONG:**
```python
# Modifying global without 'global'
count = 0

def increment():
    count = count + 1  # ERROR! UnboundLocalError
    return count

# Reading then modifying
value = 10

def update():
    print(value)  # Would work...
    value = 20    # ERROR! But this makes 'value' local
    
# Nested scope issue
def outer():
    x = 10
    def inner():
        x = x + 1  # ERROR! UnboundLocalError
        return x
    return inner()
```

**Code Example - CORRECT:**
```python
# Use 'global' keyword
count = 0

def increment():
    global count  # ✓ Declare as global
    count = count + 1
    return count

increment()  # count is now 1

# Or pass as parameter (better)
def increment(count):
    return count + 1

count = 0
count = increment(count)  # ✓ Better approach

# Return new value instead
value = 10

def update(val):
    return val + 10

value = update(value)  # ✓ Functional approach

# Use nonlocal for nested functions
def outer():
    x = 10
    def inner():
        nonlocal x  # ✓ For nested scope
        x = x + 1
        return x
    return inner()

# Avoid global when possible
# Instead, use class or pass parameters
class Counter:
    def __init__(self):
        self.count = 0
    
    def increment(self):
        self.count += 1  # ✓ No global needed

counter = Counter()
counter.increment()
```

**Scope Best Practices:**
```python
# BEST: Avoid global variables
# Use parameters and return values
def add(x, y):
    return x + y

# GOOD: Use class for state
class Calculator:
    def __init__(self):
        self.total = 0
    
    def add(self, value):
        self.total += value

# AVOID: Global variables
# global_count = 0
# def increment():
#     global global_count
#     global_count += 1
```

---

## 9.4 Default Arguments

### Using Default Parameters

```python
# Simple default
def greet(name, greeting="Hello"):
    print(f"{greeting}, {name}!")

greet("Alice")  # Uses default: "Hello, Alice!"
greet("Bob", "Hi")  # Custom: "Hi, Bob!"

# Multiple defaults
def create_user(name, age=None, city="Unknown"):
    print(f"{name}, {age}, {city}")

create_user("Alice")  # Alice, None, Unknown
create_user("Bob", 25)  # Bob, 25, Unknown
create_user("Charlie", 30, "NYC")  # Charlie, 30, NYC
```

---

### Error Type 4: Mutable Default Arguments

**What Happened:**
Using mutable objects (list, dict) as default arguments causes unexpected behavior.

**Code Example - WRONG:**
```python
# DANGEROUS: Mutable default
def add_item(item, items=[]):
    items.append(item)
    return items

list1 = add_item("apple")  # ['apple']
list2 = add_item("banana")  # ['apple', 'banana'] - Unexpected!
list3 = add_item("cherry")  # ['apple', 'banana', 'cherry'] - Wrong!
# All share the same list!

# Same with dictionaries
def add_key(key, value, data={}):
    data[key] = value
    return data

dict1 = add_key("a", 1)  # {'a': 1}
dict2 = add_key("b", 2)  # {'a': 1, 'b': 2} - Unexpected!
```

**Code Example - CORRECT:**
```python
# Use None as default, create new inside
def add_item(item, items=None):
    if items is None:
        items = []  # ✓ Create new list each time
    items.append(item)
    return items

list1 = add_item("apple")  # ['apple'] ✓
list2 = add_item("banana")  # ['banana'] ✓
list3 = add_item("cherry")  # ['cherry'] ✓

# Same pattern for dictionaries
def add_key(key, value, data=None):
    if data is None:
        data = {}  # ✓ Create new dict each time
    data[key] = value
    return data

dict1 = add_key("a", 1)  # {'a': 1} ✓
dict2 = add_key("b", 2)  # {'b': 2} ✓

# Or explicitly pass new object
def add_item(item, items=None):
    items = items if items is not None else []
    items.append(item)
    return items

# Immutable defaults are safe
def process(value, multiplier=2):  # ✓ int is immutable
    return value * multiplier

def greet(name, prefix="Hello"):  # ✓ string is immutable
    return f"{prefix}, {name}!"
```

---

## 9.5 *args and **kwargs

### Variable Arguments

```python
# *args - variable positional arguments
def add_all(*numbers):
    return sum(numbers)

add_all(1, 2)  # 3
add_all(1, 2, 3, 4, 5)  # 15

# **kwargs - variable keyword arguments
def print_info(**info):
    for key, value in info.items():
        print(f"{key}: {value}")

print_info(name="Alice", age=25, city="NYC")

# Combining all parameter types
def complex_func(required, *args, optional="default", **kwargs):
    print(f"Required: {required}")
    print(f"Args: {args}")
    print(f"Optional: {optional}")
    print(f"Kwargs: {kwargs}")

complex_func("test", 1, 2, 3, optional="custom", key="value")
# Required: test
# Args: (1, 2, 3)
# Optional: custom
# Kwargs: {'key': 'value'}
```

---

## 9.6 Lambda Functions

### Anonymous Functions

```python
# Regular function
def square(x):
    return x ** 2

# Lambda equivalent
square = lambda x: x ** 2

# Lambda with multiple parameters
add = lambda x, y: x + y

# Lambda in sorting
pairs = [(1, 'one'), (3, 'three'), (2, 'two')]
pairs.sort(key=lambda pair: pair[0])
# [(1, 'one'), (2, 'two'), (3, 'three')]

# Lambda in map
numbers = [1, 2, 3, 4, 5]
squared = list(map(lambda x: x ** 2, numbers))
# [1, 4, 9, 16, 25]

# Lambda in filter
evens = list(filter(lambda x: x % 2 == 0, numbers))
# [2, 4]
```

---

## 9.7 Recursion

### Recursive Functions

```python
# Basic recursion
def countdown(n):
    if n <= 0:  # Base case
        print("Done!")
        return
    print(n)
    countdown(n - 1)  # Recursive call

countdown(5)  # 5, 4, 3, 2, 1, Done!

# Factorial
def factorial(n):
    if n <= 1:  # Base case
        return 1
    return n * factorial(n - 1)

factorial(5)  # 120

# Fibonacci
def fibonacci(n):
    if n <= 1:  # Base case
        return n
    return fibonacci(n - 1) + fibonacci(n - 2)

fibonacci(6)  # 8
```

---

### Error Type 5: `RecursionError: maximum recursion depth exceeded`

**Error Message:**
```python
>>> def infinite():
...     return infinite()
>>> infinite()
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "<stdin>", line 2, in infinite
  [Previous line repeated 996 more times]
RecursionError: maximum recursion depth exceeded
```

**What Happened:**
Recursive function never reaches base case.

**Why It Happens:**
- Missing base case
- Base case never reached
- Wrong recursive logic

**Code Example - WRONG:**
```python
# Missing base case
def countdown(n):
    print(n)
    countdown(n - 1)  # ERROR! Never stops

# Base case never reached
def countdown(n):
    if n == 0:  # Only checks equality
        return
    countdown(n - 2)  # ERROR! Skips 0 if n is odd

# Wrong direction
def countdown(n):
    if n == 0:
        return
    countdown(n + 1)  # ERROR! Goes up, not down
```

**Code Example - CORRECT:**
```python
# Include base case
def countdown(n):
    if n <= 0:  # ✓ Base case
        return
    print(n)
    countdown(n - 1)

# Ensure base case is reachable
def countdown(n):
    if n <= 0:  # ✓ Uses <= not ==
        return
    print(n)
    countdown(n - 1)

# Add safety limit
def countdown(n, depth=0, max_depth=1000):
    if n <= 0 or depth >= max_depth:  # ✓ Safety
        return
    print(n)
    countdown(n - 1, depth + 1, max_depth)

# Use iteration when appropriate
def countdown(n):
    while n > 0:  # ✓ Often better than recursion
        print(n)
        n -= 1
```

---

## 9.8 Practice Problems

### Problem 1: Missing Argument
```python
def greet(name, age):
    print(f"{name} is {age}")

greet("Alice")
```

<details>
<summary>Click for Answer</summary>

**Error:** `TypeError: greet() missing 1 required positional argument: 'age'`

**Fix:**
```python
def greet(name, age):
    print(f"{name} is {age}")

greet("Alice", 25)  # ✓ Provide both arguments

# Or use default
def greet(name, age=0):
    print(f"{name} is {age}")

greet("Alice")  # ✓ Uses default age
```
</details>

---

### Problem 2: Missing Return
```python
def add(a, b):
    result = a + b

total = add(3, 5)
print(total * 2)
```

<details>
<summary>Click for Answer</summary>

**Error:** `TypeError: unsupported operand type(s) for *: 'NoneType' and 'int'`

**Why:** Function returns None (no return statement)

**Fix:**
```python
def add(a, b):
    result = a + b
    return result  # ✓ Add return

total = add(3, 5)
print(total * 2)  # ✓ Works: 16
```
</details>

---

### Problem 3: Global Variable
```python
count = 0

def increment():
    count = count + 1

increment()
```

<details>
<summary>Click for Answer</summary>

**Error:** `UnboundLocalError: local variable 'count' referenced before assignment`

**Fix:**
```python
count = 0

def increment():
    global count  # ✓ Declare as global
    count = count + 1

increment()
print(count)  # 1

# Or better - use parameter
def increment(n):
    return n + 1

count = 0
count = increment(count)  # ✓ Better
```
</details>

---

## 9.9 Key Takeaways

### What You Learned

1. **Match argument count** - Provide all required arguments
2. **Always return values** - Don't forget return statement
3. **Use 'global' for global variables** - Or avoid globals
4. **Don't use mutable defaults** - Use None, create inside
5. **Include base case in recursion** - Prevent infinite recursion
6. **Pass parameters instead of globals** - Better design
7. **Check return values for None** - Before using

### Common Patterns

```python
# Pattern 1: Function with defaults
def greet(name, greeting="Hello"):
    return f"{greeting}, {name}!"

# Pattern 2: Safe mutable default
def add_item(item, items=None):
    if items is None:
        items = []
    items.append(item)
    return items

# Pattern 3: Multiple returns
def get_stats(numbers):
    return min(numbers), max(numbers)

# Pattern 4: Variable arguments
def add_all(*numbers):
    return sum(numbers)
```

### Error Summary

| Error | Cause | Prevention |
|-------|-------|------------|
| `TypeError` (args) | Wrong argument count | Match function signature |
| `TypeError` (NoneType) | Missing return | Add return statement |
| `UnboundLocalError` | Global without declaration | Use 'global' or parameters |
| `RecursionError` | No base case | Include termination condition |

---

## 9.10 Moving Forward

You now understand functions! In **Chapter 10**, we'll explore **File I/O** - reading and writing files!

