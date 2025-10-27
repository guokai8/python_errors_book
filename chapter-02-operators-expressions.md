# Chapter 2: Operators and Expressions - Mathematical and Logical Errors

## Introduction

You've mastered variables and types. Now let's explore **operators** - the symbols that let you perform operations on data (+, -, *, /, ==, etc.). Understanding operators prevents calculation errors and logic bugs.

Operators in Python:
- **Arithmetic operators**: +, -, *, /, //, %, **
- **Comparison operators**: ==, !=, <, >, <=, >=
- **Logical operators**: and, or, not
- **Assignment operators**: =, +=, -=, *=, etc.

Let's master operators by understanding their errors!

---

## 2.1 Arithmetic Operators

### Basic Math Operations

```python
# Addition
result = 5 + 3  # 8

# Subtraction
result = 10 - 4  # 6

# Multiplication
result = 3 * 4  # 12

# Division
result = 10 / 3  # 3.333... (always returns float)

# Floor division (rounds down)
result = 10 // 3  # 3 (integer division)

# Modulo (remainder)
result = 10 % 3  # 1

# Exponentiation
result = 2 ** 3  # 8 (2 to the power of 3)
```

---

### Error Type 1: `ZeroDivisionError: division by zero`

**Error Message:**
```python
>>> result = 10 / 0
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ZeroDivisionError: division by zero
```

**What Happened:**
You tried to divide by zero, which is mathematically undefined.

**Why It Happens:**
- Literal zero in denominator
- Variable that becomes zero
- User input that's zero
- Calculation result that's zero

**Code Example - WRONG:**
```python
# Direct division by zero
result = 10 / 0  # ERROR!

# Variable is zero
denominator = 0
result = 100 / denominator  # ERROR!

# User input
number = int(input("Enter divisor: "))  # User enters 0
result = 50 / number  # ERROR!

# Calculation results in zero
x = 5
y = 5
result = 10 / (x - y)  # ERROR! (5 - 5 = 0)

# Modulo by zero
remainder = 10 % 0  # ERROR! Also causes ZeroDivisionError
```

**Code Example - CORRECT:**
```python
# Check before dividing
denominator = 0
if denominator != 0:
    result = 100 / denominator
else:
    result = 0  # Or handle appropriately
    print("Cannot divide by zero")

# Using try/except
try:
    result = 10 / denominator
except ZeroDivisionError:
    print("Error: Division by zero")
    result = None

# Function with validation
def safe_divide(numerator, denominator):
    """Safely divide two numbers"""
    if denominator == 0:
        return None  # Or raise custom error
    return numerator / denominator

result = safe_divide(10, 0)  # Returns None
result = safe_divide(10, 2)  # Returns 5.0

# Using a default value
def divide_with_default(a, b, default=0):
    """Divide with default value if b is zero"""
    try:
        return a / b
    except ZeroDivisionError:
        return default

result = divide_with_default(10, 0, default=0)  # 0
result = divide_with_default(10, 2)  # 5.0

# For modulo
def safe_modulo(a, b):
    """Safe modulo operation"""
    if b == 0:
        return None
    return a % b
```

**Prevention Pattern:**
```python
# Always validate divisor
def calculate_average(total, count):
    if count == 0:
        return 0  # Or appropriate default
    return total / count

# Check in mathematical expressions
x = 10
y = 5
denominator = (x - y)
if denominator != 0:
    result = 100 / denominator
```

---

### Error Type 2: `TypeError: unsupported operand type(s) for X`

**Error Message:**
```python
>>> result = "5" + 5
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: unsupported operand type(s) for +: 'str' and 'int'
```

**What Happened:**
You tried to perform an arithmetic operation on incompatible types.

**Why It Happens:**
- Mixing strings and numbers
- Using operators on wrong types
- Forgetting type conversions

**Code Example - WRONG:**
```python
# String + number
result = "5" + 5  # ERROR!

# String * string
result = "hello" * "world"  # ERROR!

# List + number
result = [1, 2, 3] + 5  # ERROR!

# Division with strings
result = "10" / "2"  # ERROR!

# Subtraction with strings
result = "10" - 5  # ERROR!
```

**Code Example - CORRECT:**
```python
# Convert string to number
result = int("5") + 5  # ✓ 10
result = float("5.5") + 5  # ✓ 10.5

# Or convert number to string
result = "5" + str(5)  # ✓ "55"

# String repetition (works!)
result = "hello" * 3  # ✓ "hellohellohello"

# List repetition (works!)
result = [1, 2] * 3  # ✓ [1, 2, 1, 2, 1, 2]

# List concatenation
result = [1, 2, 3] + [4, 5]  # ✓ [1, 2, 3, 4, 5]

# Proper string operations
result = int("10") / int("2")  # ✓ 5.0

# Type checking before operation
value = "5"
if isinstance(value, str):
    value = int(value)
result = value + 5  # ✓ 10
```

**Understanding Operator Compatibility:**
```python
# What works with each operator:

# + (Addition/Concatenation)
5 + 3  # ✓ Numbers
"a" + "b"  # ✓ Strings
[1] + [2]  # ✓ Lists
# "5" + 5  # ✗ String + Number

# * (Multiplication/Repetition)
5 * 3  # ✓ Numbers
"a" * 3  # ✓ String repetition
[1] * 3  # ✓ List repetition
# "a" * "b"  # ✗ String * String

# - (Subtraction)
5 - 3  # ✓ Numbers only
# "5" - "3"  # ✗ Not for strings

# / (Division)
10 / 2  # ✓ Numbers only
# "10" / "2"  # ✗ Not for strings
```

---

## 2.2 Comparison Operators

### Understanding Comparisons

```python
# Equality
5 == 5  # True
"hello" == "hello"  # True

# Inequality
5 != 3  # True

# Greater than / Less than
10 > 5  # True
3 < 7  # True

# Greater than or equal / Less than or equal
5 >= 5  # True
3 <= 3  # True

# Chaining comparisons
1 < 5 < 10  # True (same as: 1 < 5 and 5 < 10)
```

---

### Error Type 3: Wrong Comparison Operator

**What Happened:**
Using = instead of ==, or other comparison mistakes.

**Code Example - WRONG:**
```python
# Using = instead of ==
if x = 5:  # ERROR! SyntaxError
    print("Five")

# Comparing with wrong type
if "5" == 5:  # False (no error, but wrong logic)
    print("Equal")  # Doesn't print

# Using is for value comparison
x = 1000
y = 1000
if x is y:  # False (checks identity, not value)
    print("Same")  # Doesn't print

# Wrong negation
if not x == 5:  # Works but verbose
    print("Not five")
```

**Code Example - CORRECT:**
```python
# Use == for comparison
x = 5
if x == 5:  # ✓ Correct
    print("Five")

# Convert types before comparing
if int("5") == 5:  # ✓ True
    print("Equal")

# Use == for value comparison
x = 1000
y = 1000
if x == y:  # ✓ True
    print("Same")

# Use is only for None, True, False
value = None
if value is None:  # ✓ Correct
    print("No value")

# Use != for not equal
if x != 5:  # ✓ Better than: not x == 5
    print("Not five")

# String comparison (case-sensitive)
name = "Alice"
if name == "Alice":  # ✓ True
    print("Hello Alice")

if name.lower() == "alice":  # ✓ Case-insensitive
    print("Hello Alice")
```

**Comparison Best Practices:**
```python
# For numbers: use ==, !=, <, >, <=, >=
age = 25
if age >= 18:  # ✓
    print("Adult")

# For None: use is/is not
value = None
if value is None:  # ✓ Preferred
    print("No value")

# For strings: use ==, consider case
name = "Alice"
if name == "Alice":  # ✓ Case-sensitive
    print("Match")

if name.lower() == "alice":  # ✓ Case-insensitive
    print("Match")

# For booleans: use == or direct
is_valid = True
if is_valid:  # ✓ Direct
    print("Valid")

if is_valid == True:  # ✓ Works but verbose
    print("Valid")

# For collections: use == for value, is for identity
list1 = [1, 2, 3]
list2 = [1, 2, 3]
if list1 == list2:  # ✓ True (same values)
    print("Same values")

if list1 is list2:  # False (different objects)
    print("Same object")
```

---

## 2.3 Logical Operators

### Understanding and, or, not

```python
# and - Both must be True
True and True  # True
True and False  # False
False and False  # False

# or - At least one must be True
True or False  # True
False or False  # False
True or True  # True

# not - Reverses the boolean
not True  # False
not False  # True

# Combining operators
x = 10
if x > 5 and x < 15:  # True
    print("Between 5 and 15")

# Short-circuit evaluation
if x > 5 and expensive_function():  # expensive_function only called if x > 5
    print("Both conditions true")
```

---

### Error Type 4: Logical Operator Mistakes

**Code Example - WRONG:**
```python
# Using bitwise operators instead of logical
if True & False:  # Using & instead of 'and'
    print("Wrong operator")

# Wrong order of operations
if not x == 5:  # Confusing - applies 'not' first
    print("Not five")

# Chaining with wrong logic
age = 25
if age > 18 and < 65:  # ERROR! SyntaxError
    print("Working age")

# Missing parentheses
if x > 5 and y > 3 or z > 10:  # Ambiguous
    print("Condition met")

# Comparing with boolean literals unnecessarily
if is_valid == True:  # Verbose
    print("Valid")
```

**Code Example - CORRECT:**
```python
# Use logical operators correctly
if True and False:  # ✓ Logical AND
    print("Both true")

# Clear negation
if x != 5:  # ✓ Better than: not x == 5
    print("Not five")

# Proper chaining
age = 25
if age > 18 and age < 65:  # ✓ Correct
    print("Working age")

# Or use chaining
if 18 < age < 65:  # ✓ More Pythonic
    print("Working age")

# Use parentheses for clarity
if (x > 5 and y > 3) or z > 10:  # ✓ Clear intent
    print("Condition met")

# Direct boolean check
if is_valid:  # ✓ Simple and clear
    print("Valid")

# Combining conditions clearly
username = "admin"
password = "secret"
if username == "admin" and password == "secret":  # ✓
    print("Login successful")

# Short-circuit evaluation
if user is not None and user.is_active():  # ✓ Safe
    print("Active user")
```

**Logical Operator Truth Tables:**
```python
# AND truth table
True and True    # True
True and False   # False
False and True   # False
False and False  # False

# OR truth table
True or True     # True
True or False    # True
False or True    # True
False or False   # False

# NOT truth table
not True         # False
not False        # True

# Combining operators
not (True and False)  # True
True and not False    # True
False or not False    # True
```

---

## 2.4 Assignment Operators

### Compound Assignment

```python
# Basic assignment
x = 5

# Addition assignment
x += 3  # Same as: x = x + 3
# x is now 8

# Subtraction assignment
x -= 2  # Same as: x = x - 2
# x is now 6

# Multiplication assignment
x *= 2  # Same as: x = x * 2
# x is now 12

# Division assignment
x /= 3  # Same as: x = x / 3
# x is now 4.0

# Floor division assignment
x //= 2  # Same as: x = x // 2
# x is now 2.0

# Modulo assignment
x %= 2  # Same as: x = x % 2
# x is now 0.0

# Exponentiation assignment
x = 2
x **= 3  # Same as: x = x ** 3
# x is now 8
```

---

### Error Type 5: Assignment Mistakes

**Code Example - WRONG:**
```python
# Multiple assignment with different types
x = y = "5"
x += 5  # ERROR! TypeError: can only concatenate str (not "int") to str

# Undefined variable in compound assignment
total += 10  # ERROR! NameError if total not defined

# Wrong operator order
5 = x  # ERROR! SyntaxError: can't assign to literal

# Modifying immutable
x = 5
x[0] = 3  # ERROR! TypeError: 'int' object does not support item assignment
```

**Code Example - CORRECT:**
```python
# Initialize before compound assignment
total = 0  # ✓ Initialize first
total += 10  # ✓ Now works: total is 10

# Type-consistent operations
x = 5  # Integer
x += 3  # ✓ Still integer: 8

y = "5"  # String
y += "3"  # ✓ String concatenation: "53"

# Correct assignment order
x = 5  # ✓ Variable on left, value on right

# Multiple assignment with same type
x = y = z = 0  # ✓ All set to 0
x += 5  # ✓ x is now 5

# Counter pattern
count = 0
count += 1  # Increment
count -= 1  # Decrement

# Accumulator pattern
total = 0
numbers = [1, 2, 3, 4, 5]
for num in numbers:
    total += num  # Accumulate sum
# total is 15

# String building (though join is better for large strings)
message = ""
message += "Hello"
message += " "
message += "World"
# message is "Hello World"
```

---

## 2.5 Operator Precedence

### Order of Operations

```python
# Python operator precedence (high to low):
# 1. ** (exponentiation)
# 2. *, /, //, % (multiplication, division, floor division, modulo)
# 3. +, - (addition, subtraction)
# 4. ==, !=, <, >, <=, >= (comparisons)
# 5. not
# 6. and
# 7. or

# Examples
result = 2 + 3 * 4  # 14 (not 20) - multiplication first
result = (2 + 3) * 4  # 20 - parentheses override

result = 10 - 3 - 2  # 5 (left to right: 10-3=7, 7-2=5)
result = 2 ** 3 ** 2  # 512 (right to left: 3**2=9, 2**9=512)

# Comparison chaining
1 < 2 < 3  # True (same as: 1 < 2 and 2 < 3)

# Logical operators
True or False and False  # True (and before or)
(True or False) and False  # False (parentheses first)
```

---

### Error Type 6: Precedence Confusion

**Code Example - WRONG:**
```python
# Expecting left-to-right for all operators
result = 2 ** 3 ** 2  # 512, not 64 (exponentiation is right-to-left)

# Forgetting multiplication before addition
result = 2 + 3 * 4  # 14, not 20

# Logical operator precedence
if x > 5 or y > 3 and z > 10:  # 'and' has higher precedence
    # This is: x > 5 or (y > 3 and z > 10)
    # Not: (x > 5 or y > 3) and z > 10
    print("Condition met")

# Comparison with calculations
if 5 + 5 == 10:  # Works, but can be confusing
    print("Equal")
```

**Code Example - CORRECT:**
```python
# Use parentheses for clarity
result = 2 ** (3 ** 2)  # ✓ 512 (explicit right-to-left)
result = (2 ** 3) ** 2  # ✓ 64 (left-to-right)

result = (2 + 3) * 4  # ✓ 20 (addition first)

# Clear logical expressions
if (x > 5) or (y > 3 and z > 10):  # ✓ Explicit grouping
    print("Condition met")

# Separate calculation and comparison
sum_value = 5 + 5
if sum_value == 10:  # ✓ More readable
    print("Equal")

# Complex expressions with clear grouping
result = ((10 + 5) * 2) / (3 + 2)  # ✓ Very clear
# = (15 * 2) / 5
# = 30 / 5
# = 6.0

# Mathematical formula with proper precedence
# Formula: (a + b) / (c - d)
a, b, c, d = 10, 5, 8, 3
result = (a + b) / (c - d)  # ✓ Clear grouping
```

**Best Practice:**
```python
# When in doubt, use parentheses!
# Better to be explicit than to rely on precedence rules

# Unclear
result = 2 + 3 * 4 - 5 / 2

# Clear
result = 2 + (3 * 4) - (5 / 2)
```

---

## 2.6 Practice Problems - Fix These Errors!

### Problem 1: Division by Zero
```python
x = 10
y = 0
result = x / y
```

<details>
<summary>Click for Answer</summary>

**Error:** `ZeroDivisionError: division by zero`

**Why:** Dividing by zero

**Fix:**
```python
x = 10
y = 0

# Check before dividing
if y != 0:
    result = x / y
else:
    result = 0  # Or None, or handle error
    print("Cannot divide by zero")

# Or use try/except
try:
    result = x / y
except ZeroDivisionError:
    result = 0
    print("Cannot divide by zero")
```
</details>

---

### Problem 2: Type Mismatch
```python
age = "25"
next_year = age + 1
```

<details>
<summary>Click for Answer</summary>

**Error:** `TypeError: can only concatenate str (not "int") to str`

**Why:** Can't add string and integer

**Fix:**
```python
age = "25"
next_year = int(age) + 1  # ✓ Convert to int first
print(next_year)  # 26
```
</details>

---

### Problem 3: Wrong Comparison
```python
x = 5
if x = 5:
    print("Five")
```

<details>
<summary>Click for Answer</summary>

**Error:** `SyntaxError: invalid syntax`

**Why:** Using = (assignment) instead of == (comparison)

**Fix:**
```python
x = 5
if x == 5:  # ✓ Use == for comparison
    print("Five")
```
</details>

---

### Problem 4: Operator Precedence
```python
result = 10 - 5 - 3
print(result)  # What does this print?
```

<details>
<summary>Click for Answer</summary>

**Answer:** `2`

**Explanation:** Subtraction is left-to-right
- 10 - 5 = 5
- 5 - 3 = 2

**Not:** 10 - (5 - 3) = 10 - 2 = 8

**To get 8:**
```python
result = 10 - (5 - 3)  # ✓ Use parentheses
```
</details>

---

### Problem 5: Logical Operators
```python
age = 25
if age > 18 and < 65:
    print("Working age")
```

<details>
<summary>Click for Answer</summary>

**Error:** `SyntaxError: invalid syntax`

**Why:** Missing variable in second comparison

**Fix:**
```python
age = 25

# Option 1: Repeat variable
if age > 18 and age < 65:  # ✓
    print("Working age")

# Option 2: Chaining (more Pythonic)
if 18 < age < 65:  # ✓
    print("Working age")
```
</details>

---

### Problem 6: String Multiplication
```python
result = "hello" * "3"
```

<details>
<summary>Click for Answer</summary>

**Error:** `TypeError: can't multiply sequence by non-int of type 'str'`

**Why:** Can't multiply string by string

**Fix:**
```python
result = "hello" * 3  # ✓ Multiply by integer
print(result)  # "hellohellohello"

# If "3" is user input:
count = "3"
result = "hello" * int(count)  # ✓ Convert to int first
```
</details>

---

## 2.7 Key Takeaways

### What You Learned

1. **Check for zero before division** - Prevent ZeroDivisionError
2. **Match types in operations** - Convert when needed
3. **Use == for comparison** - Not = (assignment)
4. **Understand operator precedence** - Use parentheses when unclear
5. **Use logical operators correctly** - and, or, not
6. **Initialize before compound assignment** - total += 1 needs total to exist
7. **Use is for None/True/False** - Use == for other values

### Common Patterns

```python
# Pattern 1: Safe division
if denominator != 0:
    result = numerator / denominator

# Pattern 2: Type conversion
value = int(string_value) + 5

# Pattern 3: Range checking
if 0 <= value <= 100:
    # Value in range

# Pattern 4: Null checking with logic
if user is not None and user.is_active():
    # Safe to call method

# Pattern 5: Counter
count = 0
count += 1
```

### Error Summary Table

| Error Type | Common Cause | Prevention |
|------------|--------------|------------|
| `ZeroDivisionError` | Dividing by zero | Check denominator != 0 |
| `TypeError` in operations | Mixing incompatible types | Convert types first |
| `SyntaxError` in if | Using = instead of == | Use == for comparison |
| Wrong results | Operator precedence | Use parentheses |

---

## 2.8 Moving Forward

You now understand operators and expressions. You can:
- Perform arithmetic safely
- Compare values correctly
- Use logical operators effectively
- Avoid common operator errors

In **Chapter 3**, we'll explore **Strings and String Methods** - working with text and avoiding string errors!
