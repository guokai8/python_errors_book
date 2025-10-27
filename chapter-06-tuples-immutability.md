# Chapter 6: Tuples and Immutability - Understanding Immutable Sequences

## Introduction

You've mastered lists and dictionaries. Now let's explore **tuples** - immutable sequences that look similar to lists but behave very differently.

**Tuples** are ordered collections like lists, but once created, they cannot be modified. This immutability makes them useful for data that shouldn't change and as dictionary keys.

Common errors:
- **TypeError**: Trying to modify tuples
- **AttributeError**: Using list methods on tuples
- Unpacking errors
- Index errors (similar to lists)

Let's master tuples by understanding their errors!

---

## 6.1 Tuple Basics

### Creating and Using Tuples

```python
# Creating tuples
coordinates = (10, 20)
person = ("Alice", 25, "NYC")
colors = ("red", "green", "blue")

# Single element tuple (comma is required!)
single = (5,)    # ✓ Tuple with one element
not_tuple = (5)  # Just the number 5, not a tuple!

# Empty tuple
empty = ()
empty = tuple()

# Tuple without parentheses (tuple packing)
point = 10, 20, 30  # (10, 20, 30)

# Accessing elements (like lists)
first = coordinates[0]   # 10
last = coordinates[-1]   # 20

# Tuple length
length = len(person)  # 3

# Check membership
if "Alice" in person:
    print("Found Alice")

# Slicing (returns new tuple)
subset = colors[1:]  # ('green', 'blue')
```

---

### Error Type 1: `TypeError: 'tuple' object does not support item assignment`

**Error Message:**
```python
>>> coordinates = (10, 20)
>>> coordinates[0] = 15
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: 'tuple' object does not support item assignment
```

**What Happened:**
You tried to modify a tuple. Tuples are immutable - they cannot be changed after creation.

**Why It Happens:**
- Trying to change tuple elements
- Treating tuple like a list
- Not understanding immutability
- Trying to use mutating methods

**Code Example - WRONG:**
```python
coordinates = (10, 20)

# Can't modify elements
coordinates[0] = 15  # ERROR! Tuples are immutable

# Can't delete elements
del coordinates[0]  # ERROR! Can't delete from tuple

# Can't append
coordinates.append(30)  # ERROR! No append method

# Can't extend
coordinates.extend([30, 40])  # ERROR! No extend method

# Can't remove
coordinates.remove(10)  # ERROR! No remove method

# Can't use list methods
coordinates.insert(0, 5)  # ERROR! No insert method
coordinates.pop()  # ERROR! No pop method
coordinates.reverse()  # ERROR! No reverse method
coordinates.sort()  # ERROR! No sort method
```

**Code Example - CORRECT:**
```python
coordinates = (10, 20)

# Create new tuple with changes
coordinates = (15, 20)  # ✓ New tuple

# Concatenate tuples (creates new tuple)
coordinates = coordinates + (30,)  # ✓ (15, 20, 30)

# Concatenate multiple
coordinates = coordinates + (40, 50)  # ✓ (15, 20, 30, 40, 50)

# Repeat tuples
repeated = (1, 2) * 3  # ✓ (1, 2, 1, 2, 1, 2)

# Convert to list, modify, convert back
coordinates = (10, 20, 30)
temp_list = list(coordinates)  # [10, 20, 30]
temp_list[0] = 15              # Modify list
coordinates = tuple(temp_list)  # ✓ (15, 20, 30)

# Build new tuple with comprehension
numbers = (1, 2, 3, 4, 5)
doubled = tuple(x * 2 for x in numbers)  # ✓ (2, 4, 6, 8, 10)

# Replace by slicing
coordinates = (10, 20, 30)
coordinates = (15,) + coordinates[1:]  # ✓ (15, 20, 30)

# Filter tuple
numbers = (1, 2, 3, 4, 5)
evens = tuple(x for x in numbers if x % 2 == 0)  # ✓ (2, 4)

# Delete entire tuple (can delete the variable)
coordinates = (10, 20)
del coordinates  # ✓ Deletes the variable, not just contents
```

**Why Tuples Are Immutable:**
```python
# Immutability benefits:
# 1. Can be used as dictionary keys
location_data = {
    (10, 20): "Point A",
    (30, 40): "Point B"
}  # ✓ Tuples work as keys

# 2. Safer - can't be accidentally modified
def process_data(data):
    # If data is a tuple, we know it won't change
    return data[0] + data[1]

# 3. Slightly faster than lists
# 4. Can be used in sets
points = {(0, 0), (1, 1), (2, 2)}  # ✓ Set of tuples

# Lists can't do these:
# {[0, 0], [1, 1]}  # ERROR! Lists can't be in sets
# {[10, 20]: "value"}  # ERROR! Lists can't be dict keys
```

---

## 6.2 Tuple Unpacking

### Understanding Unpacking

```python
# Basic unpacking
coordinates = (10, 20)
x, y = coordinates  # x=10, y=20

# Multiple values
person = ("Alice", 25, "NYC")
name, age, city = person

# Swap values (elegant!)
a, b = 5, 10
a, b = b, a  # ✓ a=10, b=5

# Function returning multiple values
def get_coordinates():
    return 10, 20  # Returns tuple (10, 20)

x, y = get_coordinates()

# Using * to capture remaining
numbers = (1, 2, 3, 4, 5)
first, *rest = numbers  # first=1, rest=[2,3,4,5]
*start, last = numbers  # start=[1,2,3,4], last=5
first, *middle, last = numbers  # first=1, middle=[2,3,4], last=5
```

---

### Error Type 2: `ValueError: too many values to unpack` or `not enough values to unpack`

**Error Message:**
```python
>>> coordinates = (10, 20, 30)
>>> x, y = coordinates
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ValueError: too many values to unpack (expected 2)
```

**What Happened:**
The number of variables doesn't match the number of items in the tuple.

**Why It Happens:**
- Too few variables for tuple items
- Too many variables for tuple items
- Wrong assumptions about tuple size
- Function returns different number of values

**Code Example - WRONG:**
```python
# Too many values in tuple
coordinates = (10, 20, 30)
x, y = coordinates  # ERROR! 3 values, 2 variables

# Too few values in tuple
coordinates = (10,)
x, y = coordinates  # ERROR! 1 value, 2 variables

# Wrong assumption about function return
def get_data():
    return 1, 2, 3

a, b = get_data()  # ERROR! Returns 3, expecting 2

# Empty tuple
data = ()
x, y = data  # ERROR! No values to unpack
```

**Code Example - CORRECT:**
```python
# Match number of variables to tuple size
coordinates = (10, 20, 30)
x, y, z = coordinates  # ✓ 3 values, 3 variables

# Use underscore for unwanted values
x, _, z = coordinates  # ✓ Ignore middle value (y)

# Use * to capture remaining
first, *rest = coordinates  # ✓ first=10, rest=[20,30]

# Check length before unpacking
coordinates = (10, 20, 30)
if len(coordinates) == 2:
    x, y = coordinates
elif len(coordinates) == 3:
    x, y, z = coordinates

# Safe unpacking with try/except
try:
    x, y = coordinates
except ValueError:
    print("Wrong number of values")
    x, y = coordinates[0], coordinates[1]

# Unpack with defaults
def safe_unpack(tup, count, default=None):
    """Safely unpack tuple with defaults"""
    result = list(tup) + [default] * (count - len(tup))
    return tuple(result[:count])

coordinates = (10, 20)
x, y, z = safe_unpack(coordinates, 3, default=0)  # ✓ x=10, y=20, z=0

# Use indexing if unsure
coordinates = (10, 20, 30)
x = coordinates[0] if len(coordinates) > 0 else None
y = coordinates[1] if len(coordinates) > 1 else None

# Unpack only what you need
coordinates = (10, 20, 30, 40, 50)
x, y, *_ = coordinates  # ✓ Get first two, ignore rest

# Function with flexible return
def get_data():
    return 1, 2, 3

# Capture all with *
a, *rest = get_data()  # ✓ a=1, rest=[2,3]

# Or match exactly
a, b, c = get_data()  # ✓
```

**Unpacking Patterns:**
```python
# Basic unpacking
x, y = (10, 20)

# Ignore values
x, _ = (10, 20)  # Ignore second value
_, y = (10, 20)  # Ignore first value

# Extended unpacking (Python 3+)
first, *middle, last = (1, 2, 3, 4, 5)
# first=1, middle=[2,3,4], last=5

# Nested unpacking
point = ((10, 20), (30, 40))
(x1, y1), (x2, y2) = point
# x1=10, y1=20, x2=30, y2=40

# In loops
pairs = [(1, 2), (3, 4), (5, 6)]
for x, y in pairs:
    print(f"x={x}, y={y}")

# Dictionary items
person = {"name": "Alice", "age": 25}
for key, value in person.items():
    print(f"{key}: {value}")
```

---

## 6.3 Tuple Methods

### Available Tuple Methods

```python
# Tuples have only 2 methods!
numbers = (1, 2, 3, 2, 4, 2, 5)

# count() - count occurrences
count = numbers.count(2)  # 3

# index() - find first occurrence
index = numbers.index(2)  # 1 (first occurrence)
index = numbers.index(4)  # 4

# That's it! No other methods
```

---

### Error Type 3: `AttributeError: 'tuple' object has no attribute 'X'`

**Error Message:**
```python
>>> numbers = (1, 2, 3)
>>> numbers.append(4)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: 'tuple' object has no attribute 'append'
```

**What Happened:**
You tried to use a list method on a tuple.

**Why It Happens:**
- Confusing tuples with lists
- Trying to use mutating methods
- Typo in method name

**Code Example - WRONG:**
```python
numbers = (1, 2, 3)

# List methods don't work on tuples
numbers.append(4)    # ERROR! No append
numbers.extend([4])  # ERROR! No extend
numbers.insert(0, 0) # ERROR! No insert
numbers.remove(2)    # ERROR! No remove
numbers.pop()        # ERROR! No pop
numbers.sort()       # ERROR! No sort
numbers.reverse()    # ERROR! No reverse
numbers.clear()      # ERROR! No clear
```

**Code Example - CORRECT:**
```python
numbers = (1, 2, 3)

# Use the 2 available methods
count = numbers.count(2)  # ✓ Returns 1
index = numbers.index(2)  # ✓ Returns 1

# For other operations, convert to list
numbers_list = list(numbers)  # [1, 2, 3]
numbers_list.append(4)        # ✓ Works on list
numbers = tuple(numbers_list) # ✓ Convert back (1, 2, 3, 4)

# Or create new tuple
numbers = numbers + (4,)  # ✓ (1, 2, 3, 4)

# Sort: use sorted() function (returns list)
numbers = (3, 1, 4, 1, 5)
sorted_list = sorted(numbers)    # [1, 1, 3, 4, 5]
sorted_tuple = tuple(sorted(numbers))  # ✓ (1, 1, 3, 4, 5)

# Reverse: use reversed() or slicing
reversed_list = list(reversed(numbers))  # [5, 1, 4, 1, 3]
reversed_tuple = numbers[::-1]  # ✓ (5, 1, 4, 1, 3)

# Filter
numbers = (1, 2, 3, 4, 5)
evens = tuple(x for x in numbers if x % 2 == 0)  # ✓ (2, 4)

# Map
doubled = tuple(x * 2 for x in numbers)  # ✓ (2, 4, 6, 8, 10)

# Check method exists
if hasattr(numbers, 'count'):
    result = numbers.count(2)  # ✓
```

**Tuple vs List Methods:**
```python
# List has many methods:
my_list = [1, 2, 3]
my_list.append(4)      # ✓
my_list.extend([5, 6]) # ✓
my_list.insert(0, 0)   # ✓
my_list.remove(2)      # ✓
my_list.pop()          # ✓
my_list.sort()         # ✓
my_list.reverse()      # ✓
my_list.clear()        # ✓

# Tuple has only 2:
my_tuple = (1, 2, 3)
my_tuple.count(2)   # ✓
my_tuple.index(2)   # ✓
# That's all!
```

---

## 6.4 When to Use Tuples vs Lists

### Choosing the Right Collection

```python
# Use TUPLES when:
# 1. Data shouldn't change
coordinates = (10, 20)  # Position shouldn't change
date = (2025, 10, 27)   # Date is fixed

# 2. Need to use as dictionary key
locations = {
    (0, 0): "Origin",
    (10, 20): "Point A"
}

# 3. Need to use in set
points = {(0, 0), (1, 1), (2, 2)}

# 4. Returning multiple values from function
def get_stats():
    return 10, 25, 5  # min, max, avg

# 5. Data has fixed structure
person = ("Alice", 25, "NYC")  # name, age, city

# Use LISTS when:
# 1. Data will change
scores = [85, 90, 92]
scores.append(88)  # Need to add items

# 2. Need to sort/modify
numbers = [3, 1, 4, 1, 5]
numbers.sort()  # Need to sort

# 3. Collection of same type items
names = ["Alice", "Bob", "Charlie"]
names.remove("Bob")  # May need to remove

# 4. Don't need immutability
temperatures = [72, 75, 68, 70]
temperatures[0] = 73  # May need to update
```

---

## 6.5 Named Tuples

### Using Named Tuples for Clarity

```python
from collections import namedtuple

# Define named tuple type
Point = namedtuple('Point', ['x', 'y'])

# Create instance
p = Point(10, 20)

# Access by name (more readable)
print(p.x)  # 10
print(p.y)  # 20

# Or by index (still works)
print(p[0])  # 10
print(p[1])  # 20

# Still immutable
# p.x = 15  # ERROR! Can't modify

# More examples
Person = namedtuple('Person', ['name', 'age', 'city'])
person = Person('Alice', 25, 'NYC')
print(person.name)  # Alice
print(person.age)   # 25

# Unpack like regular tuple
name, age, city = person

# Convert to dict
person_dict = person._asdict()
# {'name': 'Alice', 'age': 25, 'city': 'NYC'}
```

---

## 6.6 Mutable Objects in Tuples

### Understanding Nested Mutability

```python
# Tuple itself is immutable
# But it can contain mutable objects!

data = ([1, 2, 3], [4, 5, 6])

# Can't change tuple structure
# data[0] = [7, 8, 9]  # ERROR!

# But CAN modify the lists inside
data[0].append(4)  # ✓ Works!
print(data)  # ([1, 2, 3, 4], [4, 5, 6])

# Another example
person = ("Alice", 25, ["Python", "Java"])

# Can't change tuple
# person[0] = "Bob"  # ERROR!

# But can modify the list inside
person[2].append("C++")  # ✓
print(person)  # ("Alice", 25, ["Python", "Java", "C++"])

# This is important for dictionary keys
# Tuple with mutable objects can't be dict key
skills = ["Python", "Java"]
# data = {("Alice", skills): "value"}  # ERROR if you modify skills later

# Use immutable contents for dict keys
data = {("Alice", "Python", "Java"): "value"}  # ✓
```

---

## 6.7 Practice Problems - Fix These Errors!

### Problem 1: Tuple Modification
```python
colors = ("red", "green", "blue")
colors[0] = "yellow"
```

<details>
<summary>Click for Answer</summary>

**Error:** `TypeError: 'tuple' object does not support item assignment`

**Why:** Tuples are immutable

**Fix:**
```python
colors = ("red", "green", "blue")

# Create new tuple
colors = ("yellow", "green", "blue")  # ✓

# Or use concatenation
colors = ("yellow",) + colors[1:]  # ✓
print(colors)  # ("yellow", "green", "blue")
```
</details>

---

### Problem 2: Wrong Unpacking
```python
coordinates = (10, 20, 30)
x, y = coordinates
```

<details>
<summary>Click for Answer</summary>

**Error:** `ValueError: too many values to unpack (expected 2)`

**Why:** 3 values but only 2 variables

**Fix:**
```python
coordinates = (10, 20, 30)

# Match number of variables
x, y, z = coordinates  # ✓

# Or ignore extra values
x, y, *_ = coordinates  # ✓ x=10, y=20, ignore z

# Or just take what you need
x, y = coordinates[:2]  # ✓ x=10, y=20
```
</details>

---

### Problem 3: Using List Method
```python
numbers = (1, 2, 3)
numbers.append(4)
```

<details>
<summary>Click for Answer</summary>

**Error:** `AttributeError: 'tuple' object has no attribute 'append'`

**Why:** Tuples don't have append method

**Fix:**
```python
numbers = (1, 2, 3)

# Create new tuple with concatenation
numbers = numbers + (4,)  # ✓
print(numbers)  # (1, 2, 3, 4)

# Or convert to list, modify, convert back
temp = list(numbers)
temp.append(4)
numbers = tuple(temp)  # ✓
```
</details>

---

### Problem 4: Single Element Tuple
```python
single = (5)
print(type(single))
print(len(single))
```

<details>
<summary>Click for Answer</summary>

**Issue:** This creates an int, not a tuple!

**Why:** Parentheses alone don't make tuple, need comma

**Fix:**
```python
# Need comma for single element tuple
single = (5,)  # ✓ Notice the comma
print(type(single))  # <class 'tuple'>
print(len(single))   # 1

# Or without parentheses
single = 5,  # ✓ Also works
print(type(single))  # <class 'tuple'>
```
</details>

---

### Problem 5: Empty Unpacking
```python
data = ()
x, y = data
```

<details>
<summary>Click for Answer</summary>

**Error:** `ValueError: not enough values to unpack (expected 2, got 0)`

**Why:** Empty tuple has no values

**Fix:**
```python
data = ()

# Check before unpacking
if len(data) >= 2:
    x, y = data
else:
    x, y = None, None  # ✓ Default values

# Or use try/except
try:
    x, y = data
except ValueError:
    x, y = None, None  # ✓
```
</details>

---

## 6.8 Key Takeaways

### What You Learned

1. **Tuples are immutable** - Cannot be modified after creation
2. **Only 2 methods** - count() and index()
3. **Comma makes tuple** - (5,) not (5) for single element
4. **Match unpacking variables** - Same number as tuple items
5. **Use as dict keys** - Unlike lists, tuples can be keys
6. **Create new tuples** - Use + or slicing instead of modifying
7. **Named tuples** - More readable than regular tuples

### Common Patterns

```python
# Pattern 1: Create new instead of modify
old_tuple = (1, 2, 3)
new_tuple = old_tuple + (4,)

# Pattern 2: Convert, modify, convert back
temp = list(my_tuple)
temp.append(new_item)
my_tuple = tuple(temp)

# Pattern 3: Safe unpacking
first, *rest = my_tuple

# Pattern 4: Swap values
a, b = b, a

# Pattern 5: Multiple return values
def get_data():
    return value1, value2, value3
```

### Error Summary Table

| Error Type | Common Cause | Prevention |
|------------|--------------|------------|
| `TypeError` (assignment) | Trying to modify tuple | Create new tuple instead |
| `AttributeError` | Using list methods | Only use count() and index() |
| `ValueError` (unpacking) | Wrong number of variables | Match count or use * |
| Single element | Missing comma | Use (value,) not (value) |

---

## 6.9 Moving Forward

You now understand tuples and immutability. You can:
- Use tuples appropriately
- Unpack values safely
- Understand when to use tuples vs lists
- Work with immutable data
- Use tuples as dictionary keys

In **Chapter 7**, we'll explore **Conditional Statements** - if/elif/else and logical flow!

