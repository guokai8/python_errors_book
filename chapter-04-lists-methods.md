# Chapter 4: Lists and List Methods - Collection Errors

## Introduction

You've mastered strings. Now let's explore **lists** - Python's most versatile collection type. Lists store multiple items in a single variable and are **mutable** (unlike strings).

Common list errors:
- **IndexError**: Accessing invalid positions
- **ValueError**: Item not found
- **TypeError**: Wrong operations or types
- **AttributeError**: Wrong methods

Lists are everywhere in Python. Let's master them by understanding their errors!

---

## 4.1 List Basics

### Creating and Using Lists

```python
# Creating lists
numbers = [1, 2, 3, 4, 5]
names = ["Alice", "Bob", "Charlie"]
mixed = [1, "hello", 3.14, True]
empty = []

# List length
length = len(numbers)  # 5

# Accessing elements (0-indexed)
first = numbers[0]   # 1
last = numbers[-1]   # 5

# Modifying elements (lists are mutable!)
numbers[0] = 10  # [10, 2, 3, 4, 5]

# Adding elements
numbers.append(6)  # [10, 2, 3, 4, 5, 6]

# List concatenation
combined = [1, 2] + [3, 4]  # [1, 2, 3, 4]

# List repetition
repeated = [1, 2] * 3  # [1, 2, 1, 2, 1, 2]
```

---

### Error Type 1: `IndexError: list index out of range`

**Error Message:**
```python
>>> numbers = [1, 2, 3]
>>> print(numbers[5])
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
IndexError: list index out of range
```

**What Happened:**
You tried to access an index that doesn't exist in the list.

**Why It Happens:**
- Index beyond list length
- Empty list
- Off-by-one errors
- Wrong loop bounds

**Code Example - WRONG:**
```python
numbers = [1, 2, 3]  # Length 3, indices 0-2

# Index too large
value = numbers[5]  # ERROR! Valid indices: 0-2

# Empty list
empty = []
value = empty[0]  # ERROR! No elements

# Off-by-one in loop
for i in range(len(numbers) + 1):  # Goes to 4!
    print(numbers[i])  # ERROR when i=3

# Wrong assumption
first = numbers[1]  # Gets 2, not 1 (0-based!)

# Negative index too large
value = numbers[-10]  # ERROR! Only -3 to -1 valid

# After removing elements
numbers = [1, 2, 3]
numbers.remove(2)  # Now [1, 3]
value = numbers[2]  # ERROR! Only 0-1 valid now
```

**Code Example - CORRECT:**
```python
numbers = [1, 2, 3]

# Use valid indices
first = numbers[0]   # ✓ 1
last = numbers[2]    # ✓ 3
last = numbers[-1]   # ✓ 3 (better - works for any length)

# Check length before accessing
index = 5
if index < len(numbers):
    value = numbers[index]
else:
    print(f"Index {index} out of range")

# Safe access with try/except
try:
    value = numbers[5]
except IndexError:
    value = None
    print("Index out of range")

# Use proper loop bounds
for i in range(len(numbers)):  # ✓ 0, 1, 2
    print(numbers[i])

# Better: iterate directly (no indexing needed)
for num in numbers:  # ✓ No index errors possible
    print(num)

# Safe get with default (custom function)
def safe_get(lst, index, default=None):
    """Safely get list item with default"""
    try:
        return lst[index]
    except IndexError:
        return default

value = safe_get(numbers, 5, default=0)  # ✓ Returns 0

# Check before accessing after modifications
numbers = [1, 2, 3]
numbers.remove(2)
if len(numbers) > 2:
    value = numbers[2]
else:
    print("Not enough elements")

# Use slicing (doesn't raise errors)
subset = numbers[5:10]  # ✓ Returns [], no error
```

**List Indexing Reference:**
```python
numbers = [10, 20, 30, 40, 50]
#           0   1   2   3   4   (positive indices)
#          -5  -4  -3  -2  -1   (negative indices)

numbers[0]    # 10 (first)
numbers[4]    # 50 (last)
numbers[-1]   # 50 (last, better way)
numbers[-5]   # 10 (first)

# Valid range: 0 to len(numbers)-1
# Or: -len(numbers) to -1
```

---

## 4.2 List Slicing

### Understanding List Slicing

```python
numbers = [0, 1, 2, 3, 4, 5]

# Basic slicing [start:stop]
numbers[1:4]   # [1, 2, 3] (indices 1, 2, 3)
numbers[0:3]   # [0, 1, 2]

# Omitting start or stop
numbers[:3]    # [0, 1, 2] (from beginning)
numbers[3:]    # [3, 4, 5] (to end)
numbers[:]     # [0, 1, 2, 3, 4, 5] (copy entire list)

# Step parameter [start:stop:step]
numbers[::2]   # [0, 2, 4] (every 2nd element)
numbers[1::2]  # [1, 3, 5] (every 2nd, starting at 1)

# Negative step (reverse)
numbers[::-1]  # [5, 4, 3, 2, 1, 0] (reversed)

# Negative indices in slicing
numbers[-3:]   # [3, 4, 5] (last 3 elements)
numbers[:-2]   # [0, 1, 2, 3] (all but last 2)

# Modifying with slicing
numbers[1:3] = [10, 20]  # [0, 10, 20, 3, 4, 5]
```

---

## 4.3 List Methods

### Common List Methods

```python
numbers = [1, 2, 3]

# Adding elements
numbers.append(4)        # [1, 2, 3, 4] - add to end
numbers.insert(0, 0)     # [0, 1, 2, 3, 4] - add at position
numbers.extend([5, 6])   # [0, 1, 2, 3, 4, 5, 6] - add multiple

# Removing elements
numbers.remove(3)        # [0, 1, 2, 4, 5, 6] - remove first occurrence
popped = numbers.pop()   # [0, 1, 2, 4, 5] - remove and return last
popped = numbers.pop(0)  # [1, 2, 4, 5] - remove at index

# Finding elements
index = numbers.index(2)  # 1 - position of first occurrence
count = numbers.count(2)  # 1 - how many times it appears

# Sorting
numbers.sort()            # [1, 2, 4, 5] - sort in place
numbers.reverse()         # [5, 4, 2, 1] - reverse in place

# Clearing
numbers.clear()          # [] - remove all elements

# Copying
original = [1, 2, 3]
shallow_copy = original.copy()  # Creates independent copy
```

---

### Error Type 2: `ValueError: X is not in list`

**Error Message:**
```python
>>> numbers = [1, 2, 3]
>>> numbers.remove(5)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ValueError: list.remove(x): x not in list
```

**What Happened:**
You tried to remove or find an item that doesn't exist in the list.

**Why It Happens:**
- Using remove() on non-existent item
- Using index() on non-existent item
- Wrong value or type

**Code Example - WRONG:**
```python
numbers = [1, 2, 3]

# Remove non-existent item
numbers.remove(5)  # ERROR! 5 not in list

# Find non-existent item
index = numbers.index(5)  # ERROR! 5 not in list

# Type mismatch
numbers = [1, 2, 3]
numbers.remove("1")  # ERROR! "1" (string) not same as 1 (int)

# After already removing
numbers = [1, 2, 3]
numbers.remove(2)  # [1, 3]
numbers.remove(2)  # ERROR! 2 already removed

# Case sensitivity with strings
names = ["Alice", "Bob"]
names.remove("alice")  # ERROR! Case doesn't match
```

**Code Example - CORRECT:**
```python
numbers = [1, 2, 3]

# Check before removing
if 5 in numbers:
    numbers.remove(5)
else:
    print("5 not in list")  # ✓

# Use try/except for remove
try:
    numbers.remove(5)
except ValueError:
    print("Item not found")  # ✓

# Check before finding
if 5 in numbers:
    index = numbers.index(5)
else:
    index = -1  # or None

# Safe find function
def safe_index(lst, item, default=-1):
    """Safely find index with default"""
    try:
        return lst.index(item)
    except ValueError:
        return default

index = safe_index(numbers, 5, default=-1)  # ✓ Returns -1

# Remove by index instead (if you know position)
if len(numbers) > 2:
    numbers.pop(2)  # ✓ Removes item at index 2

# Remove all occurrences
numbers = [1, 2, 3, 2, 2]
while 2 in numbers:  # ✓ Removes all 2s
    numbers.remove(2)
# Result: [1, 3]

# Or use list comprehension
numbers = [1, 2, 3, 2, 2]
numbers = [x for x in numbers if x != 2]  # ✓ [1, 3]

# Case-insensitive removal for strings
names = ["Alice", "Bob"]
to_remove = "alice"
names = [n for n in names if n.lower() != to_remove.lower()]  # ✓

# Use discard for sets (no error if not found)
number_set = {1, 2, 3}
number_set.discard(5)  # ✓ No error - we'll learn sets in Ch 5
```

---

### Error Type 3: `TypeError: list indices must be integers or slices, not X`

**Error Message:**
```python
>>> numbers = [1, 2, 3]
>>> print(numbers["0"])
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: list indices must be integers or slices, not str
```

**What Happened:**
You tried to use a non-integer as a list index.

**Why It Happens:**
- Using string instead of integer
- Using float instead of integer
- Forgetting to convert types

**Code Example - WRONG:**
```python
numbers = [1, 2, 3]

# String index
value = numbers["0"]  # ERROR! Use 0, not "0"

# Float index
value = numbers[1.5]  # ERROR! Use int, not float

# Variable that's wrong type
index = "1"
value = numbers[index]  # ERROR! index is string

# User input (always string)
index = input("Enter index: ")  # "1"
value = numbers[index]  # ERROR! Need to convert
```

**Code Example - CORRECT:**
```python
numbers = [1, 2, 3]

# Use integer indices
value = numbers[0]  # ✓ 1

# Convert string to int
index = "1"
value = numbers[int(index)]  # ✓ 2

# Convert user input
index = input("Enter index: ")
try:
    index = int(index)
    value = numbers[index]
except (ValueError, IndexError) as e:
    print(f"Invalid index: {e}")

# Convert float to int (if needed)
index = 1.7
value = numbers[int(index)]  # ✓ Uses 1

# Validate before converting
index_str = "1"
if index_str.isdigit():
    value = numbers[int(index_str)]  # ✓
else:
    print("Invalid index")

# Safe index function
def get_by_index(lst, index):
    """Safely get item by index, handling type conversion"""
    try:
        # Convert to int if needed
        if not isinstance(index, int):
            index = int(index)
        return lst[index]
    except (ValueError, IndexError, TypeError) as e:
        return None

value = get_by_index(numbers, "1")  # ✓ Returns 2
value = get_by_index(numbers, "10")  # ✓ Returns None
```

---

## 4.4 List Mutability

### Understanding Mutability

```python
# Lists are mutable - can be changed in place
numbers = [1, 2, 3]
numbers[0] = 10  # ✓ Changed: [10, 2, 3]
numbers.append(4)  # ✓ Changed: [10, 2, 3, 4]

# This affects all references
list1 = [1, 2, 3]
list2 = list1  # list2 points to SAME list
list1.append(4)
print(list1)  # [1, 2, 3, 4]
print(list2)  # [1, 2, 3, 4] - also changed!

# To avoid this, create a copy
list1 = [1, 2, 3]
list2 = list1.copy()  # or list1[:]
list1.append(4)
print(list1)  # [1, 2, 3, 4]
print(list2)  # [1, 2, 3] - unchanged ✓
```

---

### Error Type 4: Unintended List Modification

**What Happened:**
Modifying a list affects all references to that list.

**Code Example - WRONG:**
```python
# Shared reference problem
def add_item(lst):
    lst.append(4)
    return lst

original = [1, 2, 3]
new_list = add_item(original)
print(original)  # [1, 2, 3, 4] - Oops! Original changed!

# Default mutable argument (dangerous!)
def add_to_list(item, lst=[]):
    lst.append(item)
    return lst

result1 = add_to_list(1)  # [1]
result2 = add_to_list(2)  # [1, 2] - Unexpected! Same list!
result3 = add_to_list(3)  # [1, 2, 3] - Still same list!

# Multiple references
list1 = [1, 2, 3]
list2 = list1
list3 = list1
list2.append(4)
print(list1)  # [1, 2, 3, 4]
print(list2)  # [1, 2, 3, 4]
print(list3)  # [1, 2, 3, 4] - All changed!
```

**Code Example - CORRECT:**
```python
# Create a copy in function
def add_item(lst):
    new_list = lst.copy()  # ✓ Create copy
    new_list.append(4)
    return new_list

original = [1, 2, 3]
new_list = add_item(original)
print(original)  # [1, 2, 3] - Unchanged ✓
print(new_list)  # [1, 2, 3, 4]

# Use None as default, create new list inside
def add_to_list(item, lst=None):
    if lst is None:
        lst = []  # ✓ Create new list each time
    lst.append(item)
    return lst

result1 = add_to_list(1)  # [1] ✓
result2 = add_to_list(2)  # [2] ✓ Different list
result3 = add_to_list(3)  # [3] ✓ Different list

# Create independent copies
list1 = [1, 2, 3]
list2 = list1.copy()  # ✓ or list1[:]
list3 = list1.copy()  # ✓
list2.append(4)
print(list1)  # [1, 2, 3] - Unchanged
print(list2)  # [1, 2, 3, 4]
print(list3)  # [1, 2, 3] - Unchanged

# Deep copy for nested lists
import copy
nested = [[1, 2], [3, 4]]
shallow = nested.copy()  # Inner lists still shared
deep = copy.deepcopy(nested)  # ✓ Complete independent copy

# Explicitly modify or return new
def process_list(lst, modify=False):
    """
    Process list - modify in place or return new
    """
    if modify:
        lst.append(4)
        return lst
    else:
        new_list = lst.copy()
        new_list.append(4)
        return new_list

original = [1, 2, 3]
new = process_list(original, modify=False)  # ✓ Original safe
```

---

## 4.5 List Comprehensions

### Understanding List Comprehensions

```python
# Traditional loop
squares = []
for x in range(5):
    squares.append(x ** 2)
# [0, 1, 4, 9, 16]

# List comprehension (more Pythonic)
squares = [x ** 2 for x in range(5)]
# [0, 1, 4, 9, 16]

# With condition
evens = [x for x in range(10) if x % 2 == 0]
# [0, 2, 4, 6, 8]

# Transforming list
names = ["alice", "bob", "charlie"]
upper_names = [name.upper() for name in names]
# ["ALICE", "BOB", "CHARLIE"]

# Nested comprehensions
matrix = [[i*j for j in range(3)] for i in range(3)]
# [[0, 0, 0], [0, 1, 2], [0, 2, 4]]
```

---

### Error Type 5: List Comprehension Mistakes

**Code Example - WRONG:**
```python
# Syntax error - wrong order
squares = [for x in range(5) x ** 2]  # ERROR! Wrong order

# Missing brackets
squares = x ** 2 for x in range(5)  # ERROR! This makes generator, not list

# Using wrong variable
numbers = [1, 2, 3]
doubled = [x * 2 for num in numbers]  # ERROR! x not defined

# Complex logic without parentheses
result = [x for x in range(10) if x > 5 and < 8]  # ERROR! Syntax error

# Trying to use statements
result = [print(x) for x in range(5)]  # Works but prints None values
```

**Code Example - CORRECT:**
```python
# Correct syntax [expression for variable in iterable]
squares = [x ** 2 for x in range(5)]  # ✓

# Add brackets for list
squares = [x ** 2 for x in range(5)]  # ✓ List
# Or omit for generator (advanced)
squares_gen = (x ** 2 for x in range(5))  # Generator

# Use correct variable
numbers = [1, 2, 3]
doubled = [num * 2 for num in numbers]  # ✓

# Complete conditions
result = [x for x in range(10) if x > 5 and x < 8]  # ✓
# Or use chaining
result = [x for x in range(10) if 5 < x < 8]  # ✓

# Use functions for side effects separately
for x in range(5):  # ✓ Better for printing
    print(x)

# Complex comprehensions
# Filter and transform
numbers = [1, 2, 3, 4, 5, 6]
even_squares = [x ** 2 for x in numbers if x % 2 == 0]  # ✓
# [4, 16, 36]

# Multiple conditions
result = [x for x in range(20) if x % 2 == 0 if x % 3 == 0]  # ✓
# [0, 6, 12, 18]

# Nested comprehension
matrix = [[i+j for j in range(3)] for i in range(3)]  # ✓
# [[0, 1, 2], [1, 2, 3], [2, 3, 4]]

# Flattening nested list
nested = [[1, 2], [3, 4], [5, 6]]
flat = [item for sublist in nested for item in sublist]  # ✓
# [1, 2, 3, 4, 5, 6]
```

---

## 4.6 Common List Patterns

### Useful List Operations

```python
numbers = [1, 2, 3, 4, 5]

# Check if list is empty
if numbers:  # ✓ True if not empty
    print("Has items")

if not numbers:  # ✓ True if empty
    print("Empty")

# Check membership
if 3 in numbers:  # ✓ True
    print("Found 3")

# Get min/max
minimum = min(numbers)  # 1
maximum = max(numbers)  # 5

# Sum all elements
total = sum(numbers)  # 15

# Count occurrences
numbers = [1, 2, 2, 3, 2]
count = numbers.count(2)  # 3

# Find all indices of value
indices = [i for i, x in enumerate(numbers) if x == 2]
# [1, 2, 4]

# Remove duplicates (preserves order)
numbers = [1, 2, 2, 3, 1, 4]
unique = list(dict.fromkeys(numbers))  # [1, 2, 3, 4]
# Or using set (may not preserve order)
unique = list(set(numbers))

# Zip multiple lists
names = ["Alice", "Bob"]
ages = [25, 30]
combined = list(zip(names, ages))
# [("Alice", 25), ("Bob", 30)]

# Sort without modifying original
numbers = [3, 1, 4, 1, 5]
sorted_numbers = sorted(numbers)  # [1, 1, 3, 4, 5]
print(numbers)  # [3, 1, 4, 1, 5] - unchanged
```

---

## 4.7 Practice Problems - Fix These Errors!

### Problem 1: Index Out of Range
```python
numbers = [10, 20, 30]
for i in range(len(numbers) + 1):
    print(numbers[i])
```

<details>
<summary>Click for Answer</summary>

**Error:** `IndexError: list index out of range`

**Why:** Loop goes to index 3, but list only has indices 0-2

**Fix:**
```python
numbers = [10, 20, 30]
for i in range(len(numbers)):  # ✓ Remove +1
    print(numbers[i])

# Or better - iterate directly:
for num in numbers:  # ✓ No indexing needed
    print(num)
```
</details>

---

### Problem 2: ValueError
```python
numbers = [1, 2, 3]
numbers.remove(5)
```

<details>
<summary>Click for Answer</summary>

**Error:** `ValueError: list.remove(x): x not in list`

**Why:** 5 doesn't exist in the list

**Fix:**
```python
numbers = [1, 2, 3]

# Check first
if 5 in numbers:
    numbers.remove(5)
else:
    print("5 not in list")  # ✓

# Or use try/except
try:
    numbers.remove(5)
except ValueError:
    print("Item not found")  # ✓
```
</details>

---

### Problem 3: Type Error
```python
numbers = [1, 2, 3]
index = "1"
print(numbers[index])
```

<details>
<summary>Click for Answer</summary>

**Error:** `TypeError: list indices must be integers or slices, not str`

**Why:** Index is string, needs to be integer

**Fix:**
```python
numbers = [1, 2, 3]
index = "1"
print(numbers[int(index)])  # ✓ Convert to int
# Prints: 2
```
</details>

---

### Problem 4: Unintended Modification
```python
def add_item(lst):
    lst.append(99)
    return lst

original = [1, 2, 3]
new_list = add_item(original)
print(original)  # What does this print?
```

<details>
<summary>Click for Answer</summary>

**Issue:** Prints `[1, 2, 3, 99]` - original was modified!

**Why:** Function modified the original list, not a copy

**Fix:**
```python
def add_item(lst):
    new = lst.copy()  # ✓ Create copy
    new.append(99)
    return new

original = [1, 2, 3]
new_list = add_item(original)
print(original)  # [1, 2, 3] - unchanged ✓
print(new_list)  # [1, 2, 3, 99]
```
</details>

---

### Problem 5: Wrong List Comprehension
```python
numbers = [1, 2, 3, 4, 5]
evens = [x for x in numbers if x % 2 = 0]
```

<details>
<summary>Click for Answer</summary>

**Error:** `SyntaxError: invalid syntax`

**Why:** Using = (assignment) instead of == (comparison)

**Fix:**
```python
numbers = [1, 2, 3, 4, 5]
evens = [x for x in numbers if x % 2 == 0]  # ✓ Use ==
print(evens)  # [2, 4]
```
</details>

---

## 4.8 Key Takeaways

### What You Learned

1. **Check length before indexing** - Avoid IndexError
2. **Check membership before removing** - Use `in` operator
3. **Use integer indices** - Convert strings/floats to int
4. **Be aware of mutability** - Lists can be modified
5. **Create copies when needed** - Use .copy() or [:]
6. **Use list comprehensions** - More Pythonic and readable
7. **Iterate directly when possible** - Avoid indexing errors

### Common Patterns

```python
# Pattern 1: Safe access
if index < len(numbers):
    value = numbers[index]

# Pattern 2: Check before remove
if item in numbers:
    numbers.remove(item)

# Pattern 3: Create copy
new_list = original.copy()

# Pattern 4: Iterate without indices
for item in numbers:
    print(item)

# Pattern 5: List comprehension
result = [x * 2 for x in numbers if x > 0]

# Pattern 6: Empty check
if numbers:  # Has items
    first = numbers[0]
```

### Error Summary Table

| Error Type | Common Cause | Prevention |
|------------|--------------|------------|
| `IndexError` | Index out of range | Check len() first, use iteration |
| `ValueError` | Item not in list | Check with `in` before remove/index |
| `TypeError` | Non-integer index | Convert to int |
| Unintended modification | Shared references | Use .copy() |

---

## 4.9 Moving Forward

You now understand lists and list methods. You can:
- Access list elements safely
- Add and remove items correctly
- Use list methods properly
- Handle mutability
- Write list comprehensions

In **Chapter 5**, we'll explore **Dictionaries and Sets** - key-value pairs and unique collections!

