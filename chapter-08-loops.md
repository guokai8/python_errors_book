# Chapter 8: Loops - Iteration Errors

## Introduction

You've mastered conditionals. Now let's explore **loops** - repeating code multiple times. Loops are essential for processing collections, repeating tasks, and iterating over data.

Common errors:
- **IndexError**: Invalid indices in loops
- **StopIteration**: Iterator exhausted
- **KeyError**: Dictionary iteration errors
- Infinite loops
- Off-by-one errors

Let's master loops!

---

## 8.1 For Loop Basics

### Iterating Over Collections

```python
# Loop over list
numbers = [1, 2, 3, 4, 5]
for num in numbers:
    print(num)

# Loop over string
for char in "hello":
    print(char)

# Loop over dictionary keys
person = {"name": "Alice", "age": 25}
for key in person:
    print(key)

# Loop over dictionary items
for key, value in person.items():
    print(f"{key}: {value}")

# Loop with range
for i in range(5):  # 0, 1, 2, 3, 4
    print(i)

# Range with start and stop
for i in range(1, 6):  # 1, 2, 3, 4, 5
    print(i)

# Range with step
for i in range(0, 10, 2):  # 0, 2, 4, 6, 8
    print(i)
```

---

### Error Type 1: `IndexError` in Loops

**Error Message:**
```python
>>> numbers = [1, 2, 3]
>>> for i in range(len(numbers) + 1):
...     print(numbers[i])
1
2
3
Traceback (most recent call last):
  File "<stdin>", line 2, in <module>
IndexError: list index out of range
```

**What Happened:**
Loop index goes beyond list length.

**Why It Happens:**
- Using `range(len(list) + 1)`
- Off-by-one errors
- Modifying list during iteration
- Wrong range bounds

**Code Example - WRONG:**
```python
numbers = [1, 2, 3]

# Loop too far
for i in range(len(numbers) + 1):
    print(numbers[i])  # ERROR when i=3

# Starting at wrong index
for i in range(1, len(numbers) + 1):
    print(numbers[i])  # ERROR when i=3

# Modifying list during iteration
for i in range(len(numbers)):
    numbers.append(i)  # ERROR! Changes length during loop
    print(numbers[i])

# Wrong understanding of range
for i in range(5):
    print(numbers[i])  # ERROR if list has < 5 items
```

**Code Example - CORRECT:**
```python
numbers = [1, 2, 3]

# Correct range (no +1)
for i in range(len(numbers)):  # ✓ 0, 1, 2
    print(numbers[i])

# Better: iterate directly (no indexing)
for num in numbers:  # ✓ No index errors possible
    print(num)

# With enumerate for index and value
for i, num in enumerate(numbers):
    print(f"Index {i}: {num}")  # ✓

# Correct start index
for i in range(len(numbers)):  # ✓ Starts at 0
    print(f"Index {i}: {numbers[i]}")

# Don't modify during iteration
# Instead, iterate over copy
for num in numbers.copy():  # ✓ Iterate over copy
    numbers.append(num * 2)

# Or collect changes, apply later
to_add = []
for num in numbers:
    to_add.append(num * 2)
numbers.extend(to_add)  # ✓ Add after loop

# Check bounds
for i in range(min(5, len(numbers))):  # ✓ Safe
    print(numbers[i])
```

**Loop Best Practices:**
```python
numbers = [1, 2, 3, 4, 5]

# BEST: Direct iteration (no indexing)
for num in numbers:
    print(num)  # ✓ Simplest, safest

# GOOD: enumerate when you need index
for i, num in enumerate(numbers):
    print(f"{i}: {num}")  # ✓

# AVOID: range(len(...)) unless necessary
for i in range(len(numbers)):
    print(numbers[i])  # Works but verbose

# NEVER: range(len(...) + 1)
# for i in range(len(numbers) + 1):  # ✗ Common error
```

---

## 8.2 While Loops

### Condition-Based Iteration

```python
# Basic while loop
count = 0
while count < 5:
    print(count)
    count += 1

# While with break
while True:
    response = input("Enter 'quit' to exit: ")
    if response == "quit":
        break

# While with continue
count = 0
while count < 10:
    count += 1
    if count % 2 == 0:
        continue  # Skip even numbers
    print(count)
```

---

### Error Type 2: Infinite Loops

**What Happened:**
Loop never ends because condition stays True.

**Why It Happens:**
- Forgetting to update condition
- Wrong condition logic
- Never reaching break statement

**Code Example - WRONG:**
```python
# Forgot to update variable
count = 0
while count < 5:
    print(count)  # Prints 0 forever!
    # ERROR! Forgot: count += 1

# Wrong condition
count = 0
while count != 5:
    count += 2  # 0, 2, 4, 6, 8... never equals 5!
    print(count)  # Infinite loop!

# Condition never becomes False
while True:
    print("Forever!")  # ERROR! No break

# Break never reached
count = 0
while count < 10:
    print(count)
    if count > 20:  # Never true!
        break
    # Forgot to increment count!
```

**Code Example - CORRECT:**
```python
# Update variable in loop
count = 0
while count < 5:
    print(count)
    count += 1  # ✓ Updates condition variable

# Use correct condition
count = 0
while count < 5:  # ✓ Use <, not !=
    print(count)
    count += 2

# Include break for True loops
while True:
    response = input("Enter 'quit': ")
    if response == "quit":
        break  # ✓ Can exit

# Ensure break is reachable
count = 0
while count < 100:  # Safety limit
    print(count)
    if count >= 10:
        break  # ✓ Reachable
    count += 1

# Add safety counter
count = 0
max_iterations = 1000
while condition and count < max_iterations:
    # Loop body
    count += 1  # ✓ Safety limit

# Use for loop when count is known
for i in range(5):  # ✓ Better than while for fixed iterations
    print(i)
```

---

## 8.3 Break and Continue

### Controlling Loop Flow

```python
# break - exit loop immediately
for i in range(10):
    if i == 5:
        break  # Exit loop when i is 5
    print(i)  # Prints 0, 1, 2, 3, 4

# continue - skip to next iteration
for i in range(5):
    if i == 2:
        continue  # Skip when i is 2
    print(i)  # Prints 0, 1, 3, 4

# break in nested loops (only exits inner loop)
for i in range(3):
    for j in range(3):
        if j == 1:
            break  # Only exits inner loop
        print(f"{i},{j}")

# Using else with loops (runs if no break)
for i in range(5):
    if i == 10:
        break
else:
    print("Loop completed")  # Prints because no break
```

---

## 8.4 Iterating Over Dictionaries

### Dictionary Iteration Patterns

```python
person = {"name": "Alice", "age": 25, "city": "NYC"}

# Iterate over keys (default)
for key in person:
    print(key)

# Explicit keys
for key in person.keys():
    print(key)

# Iterate over values
for value in person.values():
    print(value)

# Iterate over key-value pairs (BEST)
for key, value in person.items():
    print(f"{key}: {value}")
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
Cannot modify dictionary size while iterating.

**Code Example - WRONG:**
```python
person = {"name": "Alice", "age": 25, "city": "NYC"}

# Deleting during iteration
for key in person:
    if key == "age":
        del person[key]  # ERROR!

# Adding during iteration
for key in person:
    if key == "name":
        person["email"] = "alice@example.com"  # ERROR!
```

**Code Example - CORRECT:**
```python
person = {"name": "Alice", "age": 25, "city": "NYC"}

# Iterate over list of keys
for key in list(person.keys()):  # ✓ Convert to list
    if key == "age":
        del person[key]

# Collect keys to delete
to_delete = []
for key in person:
    if key == "age":
        to_delete.append(key)

for key in to_delete:
    del person[key]  # ✓

# Dictionary comprehension
person = {k: v for k, v in person.items() if k != "age"}  # ✓

# Modifying values is OK
for key in person:
    person[key] = str(person[key]).upper()  # ✓ Values OK
```

---

## 8.5 Enumerate and Zip

### Advanced Iteration

```python
# enumerate - get index and value
names = ["Alice", "Bob", "Charlie"]
for i, name in enumerate(names):
    print(f"{i}: {name}")
# 0: Alice
# 1: Bob
# 2: Charlie

# enumerate with custom start
for i, name in enumerate(names, start=1):
    print(f"{i}: {name}")
# 1: Alice
# 2: Bob
# 3: Charlie

# zip - iterate over multiple lists
names = ["Alice", "Bob"]
ages = [25, 30]
for name, age in zip(names, ages):
    print(f"{name} is {age}")
# Alice is 25
# Bob is 30

# zip with different lengths (stops at shortest)
list1 = [1, 2, 3]
list2 = ['a', 'b']
for num, letter in zip(list1, list2):
    print(num, letter)
# 1 a
# 2 b
# (3 is ignored)
```

---

## 8.6 List Comprehensions vs Loops

### Comparing Approaches

```python
# Traditional loop
squares = []
for x in range(5):
    squares.append(x ** 2)
# [0, 1, 4, 9, 16]

# List comprehension (better)
squares = [x ** 2 for x in range(5)]  # ✓

# With condition
evens = []
for x in range(10):
    if x % 2 == 0:
        evens.append(x)

# Better
evens = [x for x in range(10) if x % 2 == 0]  # ✓

# When to use loops vs comprehensions:
# Use comprehension: Simple transformations, filters
# Use loop: Complex logic, multiple statements, side effects
```

---

## 8.7 Common Loop Patterns

### Useful Patterns

```python
# Sum all numbers
numbers = [1, 2, 3, 4, 5]
total = 0
for num in numbers:
    total += num
# Or: total = sum(numbers)

# Find maximum
max_val = numbers[0]
for num in numbers[1:]:
    if num > max_val:
        max_val = num
# Or: max_val = max(numbers)

# Count occurrences
items = [1, 2, 2, 3, 2, 4]
count = 0
for item in items:
    if item == 2:
        count += 1
# Or: count = items.count(2)

# Build string
words = ["Hello", "World"]
result = ""
for word in words:
    result += word + " "
# Better: result = " ".join(words)

# Filter items
numbers = [1, 2, 3, 4, 5, 6]
evens = []
for num in numbers:
    if num % 2 == 0:
        evens.append(num)
# Better: evens = [x for x in numbers if x % 2 == 0]
```

---

## 8.8 Practice Problems

### Problem 1: Index Error
```python
numbers = [1, 2, 3]
for i in range(len(numbers) + 1):
    print(numbers[i])
```

<details>
<summary>Click for Answer</summary>

**Error:** `IndexError: list index out of range`

**Fix:**
```python
numbers = [1, 2, 3]
for i in range(len(numbers)):  # ✓ Remove +1
    print(numbers[i])

# Or better:
for num in numbers:  # ✓ No indexing
    print(num)
```
</details>

---

### Problem 2: Infinite Loop
```python
count = 0
while count < 5:
    print(count)
```

<details>
<summary>Click for Answer</summary>

**Issue:** Infinite loop - count never increments

**Fix:**
```python
count = 0
while count < 5:
    print(count)
    count += 1  # ✓ Update condition variable
```
</details>

---

### Problem 3: Dictionary Modification
```python
data = {"a": 1, "b": 2, "c": 3}
for key in data:
    if key == "b":
        del data[key]
```

<details>
<summary>Click for Answer</summary>

**Error:** `RuntimeError: dictionary changed size during iteration`

**Fix:**
```python
data = {"a": 1, "b": 2, "c": 3}

# Convert keys to list
for key in list(data.keys()):  # ✓
    if key == "b":
        del data[key]

# Or use dict comprehension
data = {k: v for k, v in data.items() if k != "b"}  # ✓
```
</details>

---

## 8.9 Key Takeaways

### What You Learned

1. **Iterate directly** - for item in list (avoid indexing)
2. **Use enumerate** - When you need both index and value
3. **Update while conditions** - Prevent infinite loops
4. **Don't modify during iteration** - Use list(dict.keys())
5. **Use break/continue** - Control loop flow
6. **List comprehensions** - For simple transformations
7. **range(len())** - Usually not needed

### Common Patterns

```python
# Pattern 1: Direct iteration
for item in collection:
    process(item)

# Pattern 2: With index
for i, item in enumerate(collection):
    print(f"{i}: {item}")

# Pattern 3: Multiple lists
for x, y in zip(list1, list2):
    print(x, y)

# Pattern 4: Dictionary items
for key, value in dict.items():
    print(f"{key}: {value}")

# Pattern 5: Break on condition
for item in items:
    if condition:
        break
```

### Error Summary

| Error | Cause | Prevention |
|-------|-------|------------|
| `IndexError` | range(len()+1) | Use range(len()) or iterate directly |
| Infinite loop | No update | Update condition variable |
| `RuntimeError` | Modify dict | Use list(dict.keys()) |

---

## 8.10 Moving Forward

You now understand loops! In **Chapter 9**, we'll explore **Functions**!