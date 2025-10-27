# Chapter 19: Debugging Techniques - Finding and Fixing Errors

## Introduction

**Debugging** is the art of finding and fixing bugs. Mastering debugging techniques dramatically improves your productivity and code quality.

Topics covered:
- Print debugging
- Using debuggers (pdb)
- IDE debugging
- Logging
- Common debugging strategies

Let's master debugging!

---

## 19.1 Print Debugging

### Basic Debugging with Print

```python
# Simple print debugging
def calculate_total(items):
    total = 0
    print(f"Starting calculation with {len(items)} items")  # Debug
    for item in items:
        print(f"Processing item: {item}")  # Debug
        total += item['price']
    print(f"Final total: {total}")  # Debug
    return total

# Print variable types
value = get_value()
print(f"value = {value}, type = {type(value)}")  # Debug

# Print with context
def process(data):
    print(f"[process] Input: {data}")  # Debug with context
    result = transform(data)
    print(f"[process] Result: {result}")  # Debug
    return result

# Use repr() for detailed output
text = "hello\nworld"
print(f"text = {text!r}")  # 'hello\nworld' (shows newline)

# Temporary assertions
def divide(a, b):
    print(f"divide({a}, {b})")  # Debug
    assert b != 0, f"b is {b}"  # Debug assertion
    return a / b
```

---

## 19.2 Python Debugger (pdb)

### Using pdb

```python
import pdb

# Set breakpoint
def buggy_function(x, y):
    result = x + y
    pdb.set_trace()  # Execution pauses here
    return result * 2

# Python 3.7+: builtin breakpoint()
def buggy_function(x, y):
    result = x + y
    breakpoint()  # Easier syntax
    return result * 2

# Post-mortem debugging
try:
    buggy_operation()
except Exception:
    import pdb
    pdb.post_mortem()  # Debug at exception

# pdb commands:
# n (next) - Execute current line
# s (step) - Step into function
# c (continue) - Continue execution
# l (list) - Show code
# p variable - Print variable
# pp variable - Pretty print
# w (where) - Show stack trace
# u (up) - Move up stack
# d (down) - Move down stack
# q (quit) - Exit debugger
```

---

## 19.3 Logging for Debugging

### Strategic Logging

```python
import logging

# Configure logging
logging.basicConfig(
    level=logging.DEBUG,
    format='%(asctime)s - %(name)s - %(levelname)s - %(message)s',
    filename='debug.log'
)

logger = logging.getLogger(__name__)

def process_data(data):
    logger.debug(f"Processing {len(data)} items")
    
    for i, item in enumerate(data):
        logger.debug(f"Item {i}: {item}")
        try:
            result = transform(item)
            logger.debug(f"Transformed to: {result}")
        except Exception as e:
            logger.error(f"Failed on item {i}: {e}")
            logger.exception("Full traceback:")
    
    logger.info("Processing complete")

# Different log levels
logger.debug("Detailed debugging info")
logger.info("General information")
logger.warning("Warning message")
logger.error("Error occurred")
logger.critical("Critical error!")

# Conditional logging
if logger.isEnabledFor(logging.DEBUG):
    expensive_debug_info = compute_debug_info()
    logger.debug(f"Debug info: {expensive_debug_info}")

# Multiple loggers
user_logger = logging.getLogger('user_actions')
system_logger = logging.getLogger('system')

user_logger.info("User logged in")
system_logger.debug("System check passed")
```

---

## 19.4 Common Debugging Strategies

### Systematic Approaches

```python
# 1. Binary search / Divide and conquer
def complex_function():
    step1()
    step2()
    print("Checkpoint 1")  # Add checkpoints
    step3()
    step4()
    print("Checkpoint 2")
    step5()

# 2. Simplify inputs
# Instead of:
result = complex_function(large_data, many_params, complex_config)

# Try:
result = complex_function([1, 2, 3], simple_params, default_config)

# 3. Comment out sections
def buggy_function():
    part1()
    part2()
    # part3()  # Comment out to isolate
    # part4()
    part5()

# 4. Add assertions
def process(data):
    assert data is not None, "Data is None"
    assert len(data) > 0, f"Data is empty: {data}"
    
    result = transform(data)
    
    assert result is not None, "Result is None"
    assert isinstance(result, list), f"Result type: {type(result)}"
    
    return result

# 5. Rubber duck debugging
# Explain your code line by line (to rubber duck)
# Often reveals the bug!

# 6. Check assumptions
def divide(a, b):
    # Assumption: b is never zero
    print(f"Dividing {a} by {b}")
    print(f"b == 0? {b == 0}")  # Check assumption
    return a / b

# 7. Read error messages carefully
try:
    result = data['key']['subkey'][0]
except Exception as e:
    print(f"Error type: {type(e)}")
    print(f"Error message: {e}")
    print(f"Error args: {e.args}")
    import traceback
    traceback.print_exc()

# 8. Verify data types
def process(value):
    print(f"value: {value}")
    print(f"type: {type(value)}")
    print(f"is None: {value is None}")
    print(f"is empty: {not value}")
    print(f"len: {len(value) if hasattr(value, '__len__') else 'N/A'}")
```

---

## 19.5 Common Bug Patterns

### Recognizing Patterns

```python
# 1. Off-by-one errors
# Wrong:
for i in range(len(items) + 1):  # Goes too far!
    print(items[i])

# Correct:
for i in range(len(items)):
    print(items[i])

# 2. Mutable default arguments
# Wrong:
def add_item(item, items=[]):
    items.append(item)
    return items

# Correct:
def add_item(item, items=None):
    if items is None:
        items = []
    items.append(item)
    return items

# 3. Variable scope issues
# Wrong:
def update_global():
    count = count + 1  # UnboundLocalError

# Correct:
def update_global():
    global count
    count = count + 1

# 4. String/bytes confusion
# Wrong:
text = b"hello"
text.upper()  # Returns bytes, not string!

# Correct:
text = b"hello"
text = text.decode('utf-8')
text.upper()  # Now works

# 5. Integer division
# Python 2 vs 3:
result = 5 / 2  # 2 in Python 2, 2.5 in Python 3

# Explicit:
result = 5 // 2  # 2 (integer division)
result = 5 / 2   # 2.5 (float division)

# 6. Reference vs copy
# Wrong:
list1 = [1, 2, 3]
list2 = list1  # Reference, not copy!
list2.append(4)
print(list1)  # [1, 2, 3, 4] - modified!

# Correct:
list1 = [1, 2, 3]
list2 = list1.copy()  # Or list(list1) or list1[:]
list2.append(4)
print(list1)  # [1, 2, 3] - unchanged
```

---

## 19.6 IDE Debugging Tools

### Using IDE Debuggers

```python
# Most IDEs (PyCharm, VS Code, etc.) provide:

# 1. Breakpoints
#    - Click left of line number
#    - Code pauses when reached

# 2. Step controls
#    - Step Over: Execute line, don't enter functions
#    - Step Into: Enter function calls
#    - Step Out: Exit current function
#    - Continue: Run until next breakpoint

# 3. Variable inspection
#    - View all variables
#    - Evaluate expressions
#    - Modify values during debugging

# 4. Watch expressions
#    - Monitor specific variables
#    - Track changes

# 5. Call stack
#    - See function call history
#    - Navigate up/down stack

# 6. Conditional breakpoints
#    - Only pause when condition is True
#    - Example: i == 10

# 7. Exception breakpoints
#    - Pause on any exception
#    - Or specific exception types
```

---

## 19.7 Performance Debugging

### Finding Slow Code

```python
import time

# Simple timing
start = time.time()
slow_function()
end = time.time()
print(f"Took {end - start:.2f} seconds")

# Context manager for timing
from contextlib import contextmanager

@contextmanager
def timer(name):
    start = time.time()
    yield
    end = time.time()
    print(f"{name} took {end - start:.2f} seconds")

with timer("Database query"):
    query_database()

# Profile with timeit
import timeit

# Time a statement
time = timeit.timeit('sum(range(100))', number=10000)
print(f"Time: {time}")

# Compare approaches
time1 = timeit.timeit('[i for i in range(1000)]', number=1000)
time2 = timeit.timeit('list(range(1000))', number=1000)
print(f"List comp: {time1}, list(): {time2}")

# Use cProfile
import cProfile
import pstats

cProfile.run('expensive_function()', 'output.prof')

# Analyze results
stats = pstats.Stats('output.prof')
stats.sort_stats('cumulative')
stats.print_stats(10)  # Top 10 functions

# Line profiler (requires line_profiler package)
# @profile decorator
# kernprof -l script.py
# python -m line_profiler script.py.lprof

# Memory profiling (requires memory_profiler)
# from memory_profiler import profile
# @profile
# def my_function():
#     ...
```

---

## 19.8 Debugging Checklist

### Systematic Approach

```python
"""
When you encounter a bug:

1. ✓ Reproduce the bug
   - Can you make it happen consistently?
   - What are the exact steps?
   - What inputs cause it?

2. ✓ Isolate the problem
   - Which function/module?
   - Binary search: comment out code
   - Simplify inputs

3. ✓ Examine the error
   - Read error message carefully
   - Note the line number
   - Check the stack trace

4. ✓ Form a hypothesis
   - What do you think is wrong?
   - What should happen vs what does happen?

5. ✓ Test your hypothesis
   - Add print statements
   - Use debugger
   - Add assertions

6. ✓ Fix the bug
   - Make smallest change possible
   - Don't add features while fixing bugs

7. ✓ Verify the fix
   - Run the test case
   - Check edge cases
   - Make sure you didn't break anything else

8. ✓ Prevent regression
   - Add a test
   - Document the bug
   - Review similar code
"""
```

---

## 19.9 Debugging Tools Summary

### Tool Comparison

```python
# Print debugging
# ✓ Quick and simple
# ✓ Works everywhere
# ✗ Clutters code
# ✗ Easy to forget to remove

# pdb (Python debugger)
# ✓ Interactive
# ✓ Inspect variables
# ✗ Command line only
# ✗ Learning curve

# IDE debuggers
# ✓ Visual interface
# ✓ Easy breakpoints
# ✓ Variable inspection
# ✗ Requires IDE

# Logging
# ✓ Permanent
# ✓ Levels (debug, info, error)
# ✓ Can log to file
# ✗ Overhead

# Assertions
# ✓ Check assumptions
# ✓ Document expectations
# ✗ Can be disabled
# ✗ Crashes on failure
```

---

## 19.10 Key Takeaways

### What You Learned

1. **Use print strategically** - Add context
2. **Learn pdb basics** - n, s, c, p commands
3. **Use logging** - For production code
4. **Read errors carefully** - Stack trace has clues
5. **Isolate the problem** - Binary search
6. **Check assumptions** - Add assertions
7. **Use IDE debugger** - Visual and powerful

### Debugging Workflow

```python
# 1. Reproduce
# 2. Isolate
# 3. Understand
# 4. Fix
# 5. Test
# 6. Prevent
```

---

## 19.11 Moving Forward

You now understand debugging! In **Chapter 20**, we'll explore **Testing and Code Quality** - the final chapter!

