# Chapter 5: Dictionaries and Sets - Key-Value and Unique Collection Errors

## Introduction

You've mastered lists. Now let's explore **dictionaries** and **sets** - two powerful collection types that work differently from lists.

**Dictionaries** store key-value pairs (like a real dictionary: word → definition). **Sets** store unique items with no duplicates.

Common errors:
- **KeyError**: Accessing non-existent dictionary keys
- **TypeError**: Unhashable types as keys
- **AttributeError**: Wrong methods
- Set operation errors

Let's master these collections by understanding their errors!

---

## 5.1 Dictionary Basics

### Creating and Using Dictionaries

```python
# Creating dictionaries
person = {
    "name": "Alice",
    "age": 25,
    "city": "New York"
}

# Empty dictionary
empty = {}
# Or
empty = dict()

# Accessing values
name = person["name"]  # "Alice"
age = person["age"]    # 25

# Adding/modifying values
person["email"] = "alice@email.com"  # Add new key
person["age"] = 26                    # Modify existing

# Dictionary length
length = len(person)  # Number of key-value pairs

# Check if key exists
if "name" in person:
    print("Name exists")
```

---

### Error Type 1: `KeyError: 'key_name'`

**Error Message:**
```python
>>> person = {"name": "Alice", "age": 25}
>>> print(person["email"])
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
KeyError: 'email'
```

**What Happened:**
You tried to access a dictionary key that doesn't exist.

**Why It Happens:**
- Key doesn't exist in dictionary
- Typo in key name
- Case sensitivity
- Wrong data type for key

**Code Example - WRONG:**
```python
person = {"name": "Alice", "age": 25}

# Non-existent key
email = person["email"]  # ERROR! Key doesn't exist

# Typo in key
name = person["nane"]  # ERROR! Typo

# Case sensitivity
city = person["City"]  # ERROR! Key is "city" not "City"

# Wrong type
data = {1: "one", 2: "two"}
value = data["1"]  # ERROR! Key is int 1, not string "1"

# After deleting
person = {"name": "Alice", "age": 25}
del person["age"]
age = person["age"]  # ERROR! Already deleted

# Nested dictionary access
data = {"user": {"name": "Alice"}}
email = data["user"]["email"]  # ERROR! "email" doesn't exist
```

**Code Example - CORRECT:**
```python
person = {"name": "Alice", "age": 25}

# Check key exists before accessing
if "email" in person:
    email = person["email"]
else:
    email = None
    print("Email not found")

# Use .get() method (RECOMMENDED)
email = person.get("email")  # ✓ Returns None if not found
email = person.get("email", "no-email@example.com")  # ✓ With default

# Use try/except
try:
    email = person["email"]
except KeyError:
    email = None
    print("Key not found")

# Check spelling carefully
name = person["name"]  # ✓ Correct spelling

# Match case exactly
data = {"city": "NYC", "City": "New York"}
city = data["city"]  # ✓ "NYC"
City = data["City"]  # ✓ "New York" (different key!)

# Match key type
data = {1: "one", 2: "two"}
value = data[1]  # ✓ Use int, not string

# Safe nested access
data = {"user": {"name": "Alice"}}
email = data.get("user", {}).get("email", "N/A")  # ✓ Safe chain

# Or check each level
if "user" in data and "email" in data["user"]:
    email = data["user"]["email"]
else:
    email = "N/A"

# Use setdefault to get or create
person = {"name": "Alice"}
email = person.setdefault("email", "default@example.com")
# If "email" exists, returns its value
# If not, sets it to default and returns default
```

**Dictionary Access Patterns:**
```python
person = {"name": "Alice", "age": 25}

# Direct access (raises KeyError if missing)
name = person["name"]  # Use when key MUST exist

# .get() method (returns None or default)
email = person.get("email")  # Use when key might not exist
email = person.get("email", "N/A")  # With default value

# Check first
if "email" in person:
    email = person["email"]

# Get with default using setdefault
email = person.setdefault("email", "new@example.com")
# Sets and returns default if key doesn't exist
```

---

## 5.2 Dictionary Methods

### Common Dictionary Methods

```python
person = {"name": "Alice", "age": 25, "city": "NYC"}

# Get keys, values, items
keys = person.keys()      # dict_keys(['name', 'age', 'city'])
values = person.values()  # dict_values(['Alice', 25, 'NYC'])
items = person.items()    # dict_items([('name', 'Alice'), ...])

# Convert to lists
key_list = list(person.keys())  # ['name', 'age', 'city']

# Get with default
email = person.get("email", "N/A")  # "N/A"

# Set default if missing
person.setdefault("country", "USA")  # Adds if not exists

# Update dictionary
person.update({"email": "alice@example.com", "age": 26})

# Remove items
age = person.pop("age")        # Remove and return value
person.pop("email", None)      # Safe removal with default
del person["city"]             # Remove (raises KeyError if missing)
person.clear()                 # Remove all items

# Copy dictionary
copy = person.copy()  # Shallow copy
```

---

### Error Type 2: `TypeError: unhashable type: 'list'`

**Error Message:**
```python
>>> data = {[1, 2]: "value"}
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: unhashable type: 'list'
```

**What Happened:**
You tried to use a mutable object (like list or dict) as a dictionary key.

**Why It Happens:**
- Using list as key
- Using dict as key
- Using set as key
- Mutable objects can't be keys

**Code Example - WRONG:**
```python
# List as key
data = {[1, 2]: "value"}  # ERROR! Lists are mutable

# Dictionary as key
data = {{"a": 1}: "value"}  # ERROR! Dicts are mutable

# Set as key
data = {{1, 2}: "value"}  # ERROR! Sets are mutable

# Variable holding mutable
key = [1, 2, 3]
data = {key: "value"}  # ERROR! key is a list
```

**Code Example - CORRECT:**
```python
# Use tuple instead of list (tuples are immutable)
data = {(1, 2): "value"}  # ✓ Tuples work as keys
print(data[(1, 2)])  # "value"

# Use frozenset instead of set
data = {frozenset([1, 2]): "value"}  # ✓ Frozen sets work

# Convert list to tuple
key_list = [1, 2, 3]
data = {tuple(key_list): "value"}  # ✓ Convert to tuple

# Immutable types that work as keys:
data = {
    42: "int",              # ✓ int
    3.14: "float",          # ✓ float
    "key": "string",        # ✓ string
    (1, 2): "tuple",        # ✓ tuple
    True: "bool",           # ✓ bool
    frozenset([1]): "fs"    # ✓ frozenset
}

# Use string representation if you must use mutable
key_list = [1, 2, 3]
key = str(key_list)  # "[1, 2, 3]"
data = {key: "value"}  # ✓ String key

# Or use tuple of sorted items for dict
original = {"b": 2, "a": 1}
key = tuple(sorted(original.items()))
data = {key: "value"}  # ✓

# Store complex keys as tuples
# Instead of: {[x, y]: value}
coordinates = {(10, 20): "point1", (30, 40): "point2"}  # ✓
```

**Hashable vs Unhashable:**
```python
# HASHABLE (can be dictionary keys):
# - int, float, string, tuple, bool, frozenset
# - Immutable objects

# UNHASHABLE (cannot be dictionary keys):
# - list, dict, set
# - Mutable objects

# Test if something is hashable
try:
    hash([1, 2, 3])
except TypeError:
    print("Unhashable")  # Lists are unhashable

hash((1, 2, 3))  # ✓ Works - tuples are hashable
```

---

## 5.3 Dictionary Iteration

### Iterating Over Dictionaries

```python
person = {"name": "Alice", "age": 25, "city": "NYC"}

# Iterate over keys (default)
for key in person:
    print(key)  # name, age, city

# Explicit keys
for key in person.keys():
    print(key)

# Iterate over values
for value in person.values():
    print(value)  # Alice, 25, NYC

# Iterate over key-value pairs
for key, value in person.items():
    print(f"{key}: {value}")
# name: Alice
# age: 25
# city: NYC
```

---

### Error Type 3: `RuntimeError: dictionary changed size during iteration`

**Error Message:**
```python
>>> person = {"name": "Alice", "age": 25}
>>> for key in person:
...     if key == "age":
...         del person[key]
RuntimeError: dictionary changed size during iteration
```

**What Happened:**
You tried to modify a dictionary while iterating over it.

**Why It Happens:**
- Adding keys during iteration
- Removing keys during iteration
- Modifying dictionary size while looping

**Code Example - WRONG:**
```python
person = {"name": "Alice", "age": 25, "city": "NYC"}

# Deleting during iteration
for key in person:
    if key == "age":
        del person[key]  # ERROR! Can't modify during iteration

# Adding during iteration
for key in person:
    if key == "name":
        person["email"] = "alice@example.com"  # ERROR!

# Popping during iteration
for key in person:
    person.pop(key)  # ERROR!
```

**Code Example - CORRECT:**
```python
person = {"name": "Alice", "age": 25, "city": "NYC"}

# Create list of keys first
for key in list(person.keys()):  # ✓ Convert to list
    if key == "age":
        del person[key]

# Or collect keys to delete
keys_to_delete = []
for key in person:
    if key == "age":
        keys_to_delete.append(key)

for key in keys_to_delete:
    del person[key]  # ✓ Delete after iteration

# Use dictionary comprehension to filter
person = {"name": "Alice", "age": 25, "city": "NYC"}
person = {k: v for k, v in person.items() if k != "age"}  # ✓
# Result: {"name": "Alice", "city": "NYC"}

# Safe modification of values (not keys) is OK
for key in person:
    person[key] = str(person[key]).upper()  # ✓ Modifying values OK
# Result: {"name": "ALICE", "city": "NYC"}

# Create new dictionary with modifications
new_person = {}
for key, value in person.items():
    if key != "age":
        new_person[key] = value  # ✓

# Use copy for safe iteration
for key in person.copy():  # ✓ Iterate over copy
    if key == "age":
        del person[key]
```

---

## 5.4 Set Basics

### Creating and Using Sets

```python
# Creating sets
numbers = {1, 2, 3, 4, 5}
names = {"Alice", "Bob", "Charlie"}

# Empty set (must use set(), not {})
empty = set()  # ✓ Empty set
# empty = {}  # This creates empty dict, not set!

# Set from list (removes duplicates)
numbers = set([1, 2, 2, 3, 3, 3])  # {1, 2, 3}

# Adding elements
numbers.add(6)  # {1, 2, 3, 6}

# Removing elements
numbers.remove(2)    # Raises KeyError if not found
numbers.discard(2)   # No error if not found
popped = numbers.pop()  # Remove and return arbitrary element

# Set length
length = len(numbers)

# Check membership
if 3 in numbers:
    print("Found 3")
```

---

### Error Type 4: `KeyError` in Sets

**Error Message:**
```python
>>> numbers = {1, 2, 3}
>>> numbers.remove(5)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
KeyError: 5
```

**What Happened:**
You tried to remove an element that doesn't exist in the set.

**Why It Happens:**
- Using remove() on non-existent item
- Already removed item
- Wrong value or type

**Code Example - WRONG:**
```python
numbers = {1, 2, 3}

# Remove non-existent item
numbers.remove(5)  # ERROR! 5 not in set

# Type mismatch
numbers.remove("1")  # ERROR! "1" != 1

# Already removed
numbers.remove(2)  # OK first time
numbers.remove(2)  # ERROR! Already removed
```

**Code Example - CORRECT:**
```python
numbers = {1, 2, 3}

# Check before removing
if 5 in numbers:
    numbers.remove(5)
else:
    print("5 not in set")

# Use discard() instead (never raises error)
numbers.discard(5)  # ✓ No error if not found
numbers.discard(2)  # ✓ Removes 2
numbers.discard(2)  # ✓ No error, already gone

# Use try/except
try:
    numbers.remove(5)
except KeyError:
    print("Item not found")

# Match type
numbers = {1, 2, 3}
numbers.discard(1)  # ✓ Use int, not string

# Remove all matching items
to_remove = {2, 5, 7}
numbers = numbers - to_remove  # ✓ Creates new set
# Only removes items that exist
```

**Set Methods Comparison:**
```python
numbers = {1, 2, 3}

# remove() - raises KeyError if not found
numbers.remove(2)  # ✓ OK
numbers.remove(5)  # ✗ KeyError

# discard() - never raises error
numbers.discard(2)  # ✓ OK
numbers.discard(5)  # ✓ OK (no error)

# pop() - removes and returns arbitrary element
value = numbers.pop()  # ✓ Removes one element
# On empty set:
empty = set()
# empty.pop()  # KeyError: 'pop from an empty set'

# clear() - removes all elements
numbers.clear()  # {}, now empty
```

---

## 5.5 Set Operations

### Mathematical Set Operations

```python
set1 = {1, 2, 3, 4}
set2 = {3, 4, 5, 6}

# Union (all elements from both sets)
union = set1 | set2  # {1, 2, 3, 4, 5, 6}
union = set1.union(set2)  # Same

# Intersection (elements in both sets)
intersection = set1 & set2  # {3, 4}
intersection = set1.intersection(set2)  # Same

# Difference (elements in set1 but not set2)
difference = set1 - set2  # {1, 2}
difference = set1.difference(set2)  # Same

# Symmetric difference (elements in either but not both)
sym_diff = set1 ^ set2  # {1, 2, 5, 6}
sym_diff = set1.symmetric_difference(set2)  # Same

# Subset and superset
is_subset = {1, 2}.issubset({1, 2, 3})  # True
is_superset = {1, 2, 3}.issuperset({1, 2})  # True

# Disjoint (no common elements)
are_disjoint = {1, 2}.isdisjoint({3, 4})  # True
```

---

### Error Type 5: `TypeError: unhashable type` in Sets

**Error Message:**
```python
>>> numbers = {1, 2, [3, 4]}
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: unhashable type: 'list'
```

**What Happened:**
You tried to add a mutable object to a set.

**Why It Happens:**
- Adding list to set
- Adding dict to set
- Adding set to set
- Set elements must be immutable

**Code Example - WRONG:**
```python
# List in set
numbers = {1, 2, [3, 4]}  # ERROR! Lists are mutable

# Dict in set
data = {1, 2, {"a": 1}}  # ERROR! Dicts are mutable

# Set in set
nested = {1, 2, {3, 4}}  # ERROR! Sets are mutable

# Adding mutable element
numbers = {1, 2, 3}
numbers.add([4, 5])  # ERROR!
```

**Code Example - CORRECT:**
```python
# Use tuple instead of list
numbers = {1, 2, (3, 4)}  # ✓ Tuples are immutable
print(numbers)  # {1, 2, (3, 4)}

# Use frozenset instead of set
nested = {1, 2, frozenset([3, 4])}  # ✓ Frozen sets work
print(nested)  # {1, 2, frozenset({3, 4})}

# Convert before adding
numbers = {1, 2, 3}
to_add = [4, 5]
numbers.add(tuple(to_add))  # ✓
# {1, 2, 3, (4, 5)}

# Only immutable types work
valid_set = {
    42,              # ✓ int
    3.14,            # ✓ float
    "hello",         # ✓ string
    (1, 2),          # ✓ tuple
    True,            # ✓ bool
    frozenset([1])   # ✓ frozenset
}

# For lists of items, store as tuples
coordinates = {(0, 0), (1, 1), (2, 2)}  # ✓

# Or convert to strings if needed
items = {str([1, 2]), str([3, 4])}  # ✓
# {'[1, 2]', '[3, 4]'}
```

---

## 5.6 Common Dictionary and Set Patterns

### Useful Patterns

```python
# Counting items
words = ["apple", "banana", "apple", "cherry", "banana", "apple"]
count = {}
for word in words:
    count[word] = count.get(word, 0) + 1
# {'apple': 3, 'banana': 2, 'cherry': 1}

# Or use setdefault
count = {}
for word in words:
    count.setdefault(word, 0)
    count[word] += 1

# Or use Counter (better)
from collections import Counter
count = Counter(words)

# Grouping items
people = [
    {"name": "Alice", "age": 25},
    {"name": "Bob", "age": 30},
    {"name": "Charlie", "age": 25}
]
by_age = {}
for person in people:
    age = person["age"]
    if age not in by_age:
        by_age[age] = []
    by_age[age].append(person["name"])
# {25: ['Alice', 'Charlie'], 30: ['Bob']}

# Remove duplicates while preserving order
items = [1, 2, 2, 3, 1, 4, 3]
unique = list(dict.fromkeys(items))  # [1, 2, 3, 4]

# Merge dictionaries (Python 3.9+)
dict1 = {"a": 1, "b": 2}
dict2 = {"c": 3, "d": 4}
merged = dict1 | dict2  # {'a': 1, 'b': 2, 'c': 3, 'd': 4}

# Or use update
merged = dict1.copy()
merged.update(dict2)

# Invert dictionary (swap keys and values)
original = {"a": 1, "b": 2, "c": 3}
inverted = {v: k for k, v in original.items()}
# {1: 'a', 2: 'b', 3: 'c'}

# Find common elements
list1 = [1, 2, 3, 4, 5]
list2 = [4, 5, 6, 7, 8]
common = set(list1) & set(list2)  # {4, 5}

# Find unique elements
unique_to_list1 = set(list1) - set(list2)  # {1, 2, 3}
```

---

## 5.7 Practice Problems - Fix These Errors!

### Problem 1: KeyError
```python
person = {"name": "Alice", "age": 25}
print(person["email"])
```

<details>
<summary>Click for Answer</summary>

**Error:** `KeyError: 'email'`

**Why:** "email" key doesn't exist

**Fix:**
```python
person = {"name": "Alice", "age": 25}

# Use .get() with default
print(person.get("email", "N/A"))  # ✓ Prints: N/A

# Or check first
if "email" in person:
    print(person["email"])
else:
    print("Email not found")  # ✓
```
</details>

---

### Problem 2: Unhashable Type
```python
data = {[1, 2]: "value"}
```

<details>
<summary>Click for Answer</summary>

**Error:** `TypeError: unhashable type: 'list'`

**Why:** Lists can't be dictionary keys (they're mutable)

**Fix:**
```python
# Use tuple instead
data = {(1, 2): "value"}  # ✓ Tuples work as keys
print(data[(1, 2)])  # "value"
```
</details>

---

### Problem 3: Modifying During Iteration
```python
person = {"name": "Alice", "age": 25, "city": "NYC"}
for key in person:
    if key == "age":
        del person[key]
```

<details>
<summary>Click for Answer</summary>

**Error:** `RuntimeError: dictionary changed size during iteration`

**Why:** Can't modify dictionary size while iterating

**Fix:**
```python
person = {"name": "Alice", "age": 25, "city": "NYC"}

# Iterate over list of keys
for key in list(person.keys()):  # ✓ Convert to list
    if key == "age":
        del person[key]

# Or use dictionary comprehension
person = {k: v for k, v in person.items() if k != "age"}  # ✓
```
</details>

---

### Problem 4: Set Remove Error
```python
numbers = {1, 2, 3}
numbers.remove(5)
```

<details>
<summary>Click for Answer</summary>

**Error:** `KeyError: 5`

**Why:** 5 doesn't exist in the set

**Fix:**
```python
numbers = {1, 2, 3}

# Use discard instead (no error if not found)
numbers.discard(5)  # ✓ No error

# Or check first
if 5 in numbers:
    numbers.remove(5)
else:
    print("5 not in set")  # ✓
```
</details>

---

### Problem 5: Empty Set Creation
```python
empty = {}
empty.add(1)
```

<details>
<summary>Click for Answer</summary>

**Error:** `AttributeError: 'dict' object has no attribute 'add'`

**Why:** {} creates empty dict, not empty set

**Fix:**
```python
# Use set() for empty set
empty = set()  # ✓ Empty set
empty.add(1)   # ✓ Works
print(empty)   # {1}

# {} creates empty dictionary
empty_dict = {}  # ✓ Empty dict
empty_dict["key"] = "value"  # ✓ Works for dict
```
</details>

---

## 5.8 Key Takeaways

### What You Learned

1. **Use .get() for safe dictionary access** - Returns None instead of KeyError
2. **Only immutable types as dict keys** - Use tuples, not lists
3. **Don't modify dict during iteration** - Create list of keys first
4. **Use discard() for safe set removal** - No error if item not found
5. **Empty set needs set()** - {} creates empty dict
6. **Check membership with in** - Before accessing or removing
7. **Sets automatically remove duplicates** - Great for unique items

### Common Patterns

```python
# Pattern 1: Safe dictionary access
value = data.get("key", default_value)

# Pattern 2: Check before access
if "key" in data:
    value = data["key"]

# Pattern 3: Safe iteration modification
for key in list(data.keys()):
    if condition:
        del data[key]

# Pattern 4: Safe set removal
numbers.discard(item)  # No error

# Pattern 5: Remove duplicates
unique = list(set(items))

# Pattern 6: Count occurrences
count = {}
for item in items:
    count[item] = count.get(item, 0) + 1
```

### Error Summary Table

| Error Type | Common Cause | Prevention |
|------------|--------------|------------|
| `KeyError` (dict) | Non-existent key | Use .get() or check with in |
| `TypeError` (unhashable) | Mutable key/element | Use immutable types (tuple, frozenset) |
| `RuntimeError` | Modifying during iteration | Iterate over list(dict.keys()) |
| `KeyError` (set) | Remove non-existent item | Use discard() instead of remove() |

---

## 5.9 Moving Forward

You now understand dictionaries and sets. You can:
- Access dictionary values safely
- Use correct types for keys
- Iterate and modify safely
- Perform set operations
- Handle unique collections

In **Chapter 6**, we'll explore **Tuples and Immutability** - understanding immutable sequences!

