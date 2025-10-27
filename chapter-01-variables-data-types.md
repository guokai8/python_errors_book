# Chapter 1: Variables and Data Types - The Foundation of Python Errors

## Introduction

Welcome to your journey of mastering Python errors! Before we can understand errors, we need to understand the basics: **variables** and **data types**. Most Python errors stem from misunderstanding how Python handles data.

Think of variables as labeled boxes that store information. The **type** of information (number, text, list, etc.) determines what operations you can perform. Using the wrong type leads to errors - and that's what we'll learn to avoid!

---

## 1.1 Understanding Variables

### What is a Variable?

```python
# A variable is a name that refers to a value
name = "Alice"
age = 25
is_student = True

# Variables can change (they're "variable")
age = 26  # Changed the value
```

**Key Concepts:**
- Variables are created when you first assign a value
- Python is **dynamically typed** - you don't declare types
- Variable names are case-sensitive (`age` ≠ `Age`)
- Use descriptive names (`user_age` not `x`)

---

### Error Type 1: `NameError: name 'X' is not defined`

**Error Message:**
```python
>>> print(age)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
NameError: name 'age' is not defined
```

**What Happened:**
You tried to use a variable that doesn't exist yet.

**Why It Happens:**
- Variable was never created
- Typo in variable name
- Variable used before assignment
- Wrong scope (variable defined in function, used outside)

**Code Example - WRONG:**
```python
# Using before defining
print(username)  # ERROR! username doesn't exist yet
username = "Alice"

# Typo in variable name
user_name = "Bob"
print(user_nane)  # ERROR! Typo: 'nane' instead of 'name'

# Case sensitivity
Age = 30
print(age)  # ERROR! Python sees 'age' and 'Age' as different

# Forgetting to assign
result  # ERROR! Just naming isn't enough
result = 10  # Need to assign a value
```

**Code Example - CORRECT:**
```python
# Define before using
username = "Alice"
print(username)  # ✓ Works

# Check spelling carefully
user_name = "Bob"
print(user_name)  # ✓ Correct spelling

# Match case exactly
age = 30
print(age)  # ✓ Correct case

# Always assign a value
result = 10
print(result)  # ✓ Works
```

**Prevention Pattern:**
```python
# Check if variable exists before using
try:
    print(username)
except NameError:
    username = "Default User"
    print(username)

# Or use getattr with default for attributes
# (We'll cover this later)
```

---

## 1.2 Basic Data Types

### Numbers: int and float

```python
# Integers (whole numbers)
age = 25
year = 2025
temperature = -10

# Floats (decimal numbers)
price = 19.99
pi = 3.14159
temperature = -10.5

# Python automatically chooses the right type
x = 5      # int
y = 5.0    # float
z = 5.     # float (same as 5.0)
```

---

### Error Type 2: `TypeError: unsupported operand type(s)`

**Error Message:**
```python
>>> "5" + 5
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: can only concatenate str (not "int") to str
```

**What Happened:**
You tried to combine incompatible types (string + number).

**Why It Happens:**
- Mixing strings and numbers in operations
- Wrong type from user input (always strings)
- Forgetting to convert types

**Code Example - WRONG:**
```python
# Mixing types
age = "25"
next_year = age + 1  # ERROR! Can't add string + int

# User input is always string
user_input = input("Enter a number: ")  # Returns string
result = user_input + 10  # ERROR! String + int

# Forgetting conversion
price = "19.99"
tax = price * 0.1  # ERROR! Can't multiply string by float
```

**Code Example - CORRECT:**
```python
# Convert string to int
age = "25"
next_year = int(age) + 1  # ✓ Works: 26

# Convert user input
user_input = input("Enter a number: ")
number = int(user_input)  # Convert to int first
result = number + 10  # ✓ Works

# Convert string to float
price = "19.99"
price_float = float(price)
tax = price_float * 0.1  # ✓ Works

# Convert number to string (for concatenation)
age = 25
message = "Age: " + str(age)  # ✓ Works
# Or use f-strings (better):
message = f"Age: {age}"  # ✓ Automatic conversion
```

**Type Conversion Functions:**
```python
# String to number
int("123")      # 123 (integer)
float("3.14")   # 3.14 (float)
int("3.14")     # ERROR! Can't convert float string directly to int
int(float("3.14"))  # 3 (convert to float first, then int)

# Number to string
str(123)        # "123"
str(3.14)       # "3.14"

# Check type
type(5)         # <class 'int'>
type(5.0)       # <class 'float'>
type("5")       # <class 'str'>

isinstance(5, int)     # True
isinstance(5.0, int)   # False
isinstance(5.0, float) # True
```

---

### Error Type 3: `ValueError: invalid literal for int()`

**Error Message:**
```python
>>> int("hello")
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ValueError: invalid literal for int() with base 10: 'hello'
```

**What Happened:**
You tried to convert a string to a number, but the string doesn't contain a valid number.

**Why It Happens:**
- String contains non-numeric characters
- Empty string
- String with spaces (leading/trailing)
- User input validation issues

**Code Example - WRONG:**
```python
# Non-numeric string
age = int("twenty-five")  # ERROR! Not a number

# Empty string
value = int("")  # ERROR! Empty

# String with spaces
number = int("  42  ")  # ERROR! (Actually this works, but not always safe)

# Decimal string to int
value = int("3.14")  # ERROR! Use float first
```

**Code Example - CORRECT:**
```python
# Validate before converting
text = "hello"
if text.isdigit():
    number = int(text)
else:
    print("Not a valid number")  # ✓ Handles error

# Handle conversion errors with try/except
user_input = "invalid"
try:
    number = int(user_input)
except ValueError:
    print("Please enter a valid number")
    number = 0  # Default value

# Strip whitespace before converting
text = "  42  "
number = int(text.strip())  # ✓ Works

# Convert float string to int (two steps)
text = "3.14"
number = int(float(text))  # ✓ Works: 3

# Validate with helper function
def safe_int(text, default=0):
    """Safely convert to int with default"""
    try:
        return int(text)
    except ValueError:
        return default

value = safe_int("invalid", default=0)  # ✓ Returns 0
value = safe_int("42")  # ✓ Returns 42
```

---

## 1.3 Strings

### String Basics

```python
# Creating strings
name = "Alice"
message = 'Hello, World!'
multiline = """This is
a multiline
string"""

# String operations
greeting = "Hello" + " " + "World"  # Concatenation
repeated = "Ha" * 3  # "HaHaHa"
length = len("Python")  # 6
```

---

### Error Type 4: String Index Errors

**Error Message:**
```python
>>> text = "Hello"
>>> print(text[10])
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
IndexError: string index out of range
```

**What Happened:**
You tried to access a character position that doesn't exist.

**Why It Happens:**
- Index too large (beyond string length)
- Forgetting Python uses 0-based indexing
- Empty string

**Code Example - WRONG:**
```python
text = "Hello"  # Length is 5, indices are 0-4

# Index too large
char = text[5]  # ERROR! Valid indices: 0-4

# Wrong assumption about length
first = text[1]  # This is 'e', not 'H' (0-based!)

# Empty string
empty = ""
char = empty[0]  # ERROR! No characters
```

**Code Example - CORRECT:**
```python
text = "Hello"

# Use valid indices (0 to len-1)
first = text[0]   # ✓ 'H' (first character)
last = text[4]    # ✓ 'o' (last character)
last = text[-1]   # ✓ 'o' (negative index from end)

# Check length before accessing
if len(text) > 5:
    char = text[5]
else:
    print("Index out of range")

# Safe access with try/except
try:
    char = text[10]
except IndexError:
    char = None  # Or default value

# Use slicing (doesn't raise error)
substring = text[10:15]  # ✓ Returns empty string, no error
```

**String Indexing:**
```python
text = "Python"
#       012345  (positive indices)
#      -654321  (negative indices)

text[0]    # 'P'
text[5]    # 'n'
text[-1]   # 'n' (last character)
text[-6]   # 'P' (first character)

# Slicing [start:stop:step]
text[0:3]   # 'Pyt' (indices 0, 1, 2)
text[:3]    # 'Pyt' (from start)
text[3:]    # 'hon' (to end)
text[::2]   # 'Pto' (every 2nd character)
text[::-1]  # 'nohtyP' (reversed)
```

---

## 1.4 Booleans

### Boolean Basics

```python
# Boolean values
is_valid = True
is_empty = False

# Comparison operations create booleans
age = 25
is_adult = age >= 18  # True
is_teen = 13 <= age < 20  # True

# Logical operations
x = True
y = False
result = x and y  # False
result = x or y   # True
result = not x    # False
```

---

### Error Type 5: Type Confusion with Booleans

**What Happened:**
Treating non-boolean values as if they were boolean, or vice versa.

**Code Example - WRONG:**
```python
# Confusing truthy/falsy with boolean
value = "False"  # This is a string!
if value:
    print("This runs!")  # String "False" is truthy!

# Comparing boolean wrong way
is_valid = True
if is_valid == "True":  # Comparing bool to string
    print("Won't work")  # Never executes

# Using assignment in condition
x = 5
if x = 10:  # ERROR! Assignment, not comparison
    print("Won't work")
```

**Code Example - CORRECT:**
```python
# Use actual boolean values
value = False  # ✓ Boolean, not string

# Compare correctly
is_valid = True
if is_valid:  # ✓ Direct boolean check
    print("Valid!")

# Use == for comparison
x = 5
if x == 10:  # ✓ Comparison operator
    print("Equal to 10")

# Understand truthy/falsy
# Falsy: False, None, 0, "", [], {}, ()
# Truthy: Everything else

if "":  # Empty string is falsy
    print("Won't print")

if "text":  # Non-empty string is truthy
    print("Will print")  # ✓

# Explicit boolean conversion
text = "hello"
bool(text)  # True (non-empty string)
bool("")    # False (empty string)
bool(0)     # False
bool(42)    # True
```

---

## 1.5 None Type

### Understanding None

```python
# None represents absence of value
result = None
name = None

# Checking for None
if result is None:
    print("No result yet")

# Don't use == for None
if result == None:  # Works but not recommended
    print("Better use 'is'")

if result is None:  # ✓ Preferred way
    print("No result")
```

---

### Error Type 6: `TypeError: 'NoneType' object is not...`

**Error Message:**
```python
>>> result = None
>>> result + 5
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: unsupported operand type(s) for +: 'NoneType' and 'int'
```

**What Happened:**
You tried to use None as if it were another type (number, string, etc.).

**Why It Happens:**
- Function returns None (implicit or explicit)
- Variable not initialized properly
- Forgetting to return value from function

**Code Example - WRONG:**
```python
# Function returns None implicitly
def calculate():
    result = 5 + 3
    # Oops, forgot to return!

answer = calculate()  # Gets None
total = answer + 10  # ERROR! None + 10

# Using None in operations
value = None
doubled = value * 2  # ERROR! Can't multiply None

# Method that returns None
numbers = [3, 1, 2]
result = numbers.sort()  # sort() returns None!
print(result[0])  # ERROR! Can't index None
```

**Code Example - CORRECT:**
```python
# Always return a value from functions
def calculate():
    result = 5 + 3
    return result  # ✓ Explicit return

answer = calculate()
total = answer + 10  # ✓ Works

# Check for None before using
value = None
if value is not None:
    doubled = value * 2
else:
    doubled = 0  # Default value

# Some methods modify in-place and return None
numbers = [3, 1, 2]
numbers.sort()  # Modifies list, returns None
print(numbers[0])  # ✓ Use the modified list: 1

# Or use function that returns new value
numbers = [3, 1, 2]
sorted_numbers = sorted(numbers)  # Returns new sorted list
print(sorted_numbers[0])  # ✓ Works: 1

# Provide default value
def get_value():
    return None

result = get_value() or 0  # Use 0 if None
result = get_value() if get_value() is not None else 0  # More explicit
```

---

## 1.6 Practice Problems - Fix These Errors!

### Problem 1: NameError
```python
print(user_name)
user_name = "Alice"
```

**What's wrong?**
<details>
<summary>Click for Answer</summary>

**Error:** `NameError: name 'user_name' is not defined`

**Why:** Variable is used before it's defined.

**Fix:**
```python
user_name = "Alice"  # Define first
print(user_name)     # Then use ✓
```
</details>

---

### Problem 2: TypeError
```python
age = "25"
next_year = age + 1
print(next_year)
```

**What's wrong?**
<details>
<summary>Click for Answer</summary>

**Error:** `TypeError: can only concatenate str (not "int") to str`

**Why:** Can't add string and integer.

**Fix:**
```python
age = "25"
next_year = int(age) + 1  # Convert to int first ✓
print(next_year)  # 26
```
</details>

---

### Problem 3: ValueError
```python
user_input = "twenty-five"
age = int(user_input)
```

**What's wrong?**
<details>
<summary>Click for Answer</summary>

**Error:** `ValueError: invalid literal for int() with base 10: 'twenty-five'`

**Why:** String doesn't contain a valid number.

**Fix:**
```python
user_input = "twenty-five"
try:
    age = int(user_input)
except ValueError:
    print("Please enter a numeric value")
    age = 0  # Default value ✓
```
</details>

---

### Problem 4: IndexError
```python
text = "Hi"
print(text[5])
```

**What's wrong?**
<details>
<summary>Click for Answer</summary>

**Error:** `IndexError: string index out of range`

**Why:** Index 5 doesn't exist (string has indices 0 and 1 only).

**Fix:**
```python
text = "Hi"
if len(text) > 5:
    print(text[5])
else:
    print("Index out of range")  # ✓

# Or use negative indexing
print(text[-1])  # Last character: 'i' ✓
```
</details>

---

### Problem 5: NoneType Error
```python
def get_discount():
    discount = 0.1
    # Missing return statement

price = 100
final_price = price - get_discount()
```

**What's wrong?**
<details>
<summary>Click for Answer</summary>

**Error:** `TypeError: unsupported operand type(s) for -: 'int' and 'NoneType'`

**Why:** Function doesn't return a value (returns None implicitly).

**Fix:**
```python
def get_discount():
    discount = 0.1
    return discount  # ✓ Add return statement

price = 100
final_price = price - get_discount()  # ✓ Works now
```
</details>

---

## 1.7 Key Takeaways

### What You Learned

1. **Define variables before using them** - Avoid `NameError`
2. **Match types in operations** - Convert when needed
3. **Validate before converting** - Use try/except for conversions
4. **Check string length before indexing** - Avoid `IndexError`
5. **Always return values from functions** - Avoid `NoneType` errors
6. **Use appropriate comparison** - `is` for None, `==` for values

### Common Patterns

```python
# Pattern 1: Safe type conversion
try:
    number = int(user_input)
except ValueError:
    number = 0  # Default value

# Pattern 2: Safe string indexing
if len(text) > index:
    char = text[index]

# Pattern 3: Check for None
if value is not None:
    # Use value

# Pattern 4: Provide defaults
result = function() or default_value
```

### Error Summary Table

| Error Type | Common Cause | Prevention |
|------------|--------------|------------|
| `NameError` | Using undefined variable | Define before use |
| `TypeError` | Mixing incompatible types | Convert types explicitly |
| `ValueError` | Invalid conversion | Validate before converting |
| `IndexError` | Invalid string index | Check length first |
| `NoneType` errors | Using None in operations | Check for None, return values |

---

## 1.8 Moving Forward

You now understand the basics of variables and data types, and how to avoid common errors. In **Chapter 2**, we'll explore **Operators and Expressions** and learn about more error types!

