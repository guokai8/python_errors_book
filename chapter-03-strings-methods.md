# Chapter 3: Strings and String Methods - Text Manipulation Errors

## Introduction

You've mastered variables and operators. Now let's explore **strings** - one of the most commonly used data types in Python. Strings represent text, and Python provides powerful methods to manipulate them.

String-related errors are extremely common:
- **IndexError**: Accessing invalid string positions
- **AttributeError**: Using wrong methods or typos
- **ValueError**: Invalid operations on strings
- **TypeError**: Wrong types in string operations

Let's master strings by understanding their errors!

---

## 3.1 String Basics

### Creating and Using Strings

```python
# Creating strings
name = "Alice"
message = 'Hello, World!'
multiline = """This is
a multiline
string"""

# String concatenation
greeting = "Hello" + " " + "World"  # "Hello World"

# String repetition
repeated = "Ha" * 3  # "HaHaHa"

# String length
length = len("Python")  # 6

# Accessing characters (0-indexed)
text = "Python"
first = text[0]   # 'P'
last = text[-1]   # 'n'
```

---

### Error Type 1: `IndexError: string index out of range`

**Error Message:**
```python
>>> text = "Hello"
>>> char = text[10]
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
IndexError: string index out of range
```

**What Happened:**
You tried to access a character at an index that doesn't exist in the string.

**Why It Happens:**
- Index beyond string length
- Empty string
- Off-by-one errors (forgetting 0-based indexing)
- Negative index too large

**Code Example - WRONG:**
```python
text = "Hello"  # Length 5, indices 0-4

# Index too large
char = text[5]  # ERROR! Valid indices: 0-4

# Much too large
char = text[100]  # ERROR!

# Empty string
empty = ""
char = empty[0]  # ERROR! No characters

# Wrong assumption
first = text[1]  # Gets 'e', not 'H' (0-based indexing!)

# Negative index too large
char = text[-10]  # ERROR! Only goes back to -5
```

**Code Example - CORRECT:**
```python
text = "Hello"

# Use valid indices
first = text[0]   # ✓ 'H' (first character)
last = text[4]    # ✓ 'o' (last character)
last = text[-1]   # ✓ 'o' (last character, better way)

# Check length before accessing
index = 10
if index < len(text):
    char = text[index]
else:
    print(f"Index {index} out of range")

# Safe access with try/except
try:
    char = text[10]
except IndexError:
    char = None  # Or default value
    print("Index out of range")

# Use slicing (doesn't raise error)
substring = text[10:15]  # ✓ Returns empty string, no error

# Get last character safely
if text:  # Check if not empty
    last = text[-1]
else:
    last = None

# Iterate safely with indices
for i in range(len(text)):
    char = text[i]  # ✓ Always valid
    print(char)

# Or iterate directly (better)
for char in text:  # ✓ No indexing needed
    print(char)
```

**String Indexing Reference:**
```python
text = "Python"
#       012345  (positive indices)
#      -654321  (negative indices)

text[0]    # 'P' (first)
text[5]    # 'n' (last)
text[-1]   # 'n' (last, using negative)
text[-6]   # 'P' (first, using negative)

# Valid range: 0 to len(text)-1
# Or: -len(text) to -1
```

---

## 3.2 String Slicing

### Understanding Slicing

```python
text = "Python"

# Basic slicing [start:stop]
text[0:3]   # 'Pyt' (indices 0, 1, 2)
text[2:5]   # 'tho' (indices 2, 3, 4)

# Omitting start or stop
text[:3]    # 'Pyt' (from beginning)
text[3:]    # 'hon' (to end)
text[:]     # 'Python' (entire string)

# Step parameter [start:stop:step]
text[::2]   # 'Pto' (every 2nd character)
text[1::2]  # 'yhn' (every 2nd, starting at 1)

# Negative step (reverse)
text[::-1]  # 'nohtyP' (reversed)

# Negative indices in slicing
text[-3:]   # 'hon' (last 3 characters)
text[:-2]   # 'Pyth' (all but last 2)
```

---

### Error Type 2: Slicing Mistakes (No Error, But Wrong Results)

**What Happened:**
Slicing doesn't raise errors, but wrong indices give unexpected results.

**Code Example - WRONG LOGIC:**
```python
text = "Hello World"

# Getting empty string unexpectedly
substring = text[5:3]  # '' (start > stop gives empty)

# Wrong order
substring = text[10:0]  # '' (should be text[0:10])

# Confusing positive and negative
substring = text[-1:0]  # '' (wrong direction)

# Off-by-one errors
# Want "Hello" (first 5 chars)
substring = text[0:4]  # 'Hell' (missing last char!)

# Want "World" (last 5 chars)
substring = text[6:10]  # 'Worl' (missing last char!)
```

**Code Example - CORRECT:**
```python
text = "Hello World"

# Get first N characters
first_5 = text[:5]  # ✓ 'Hello'

# Get last N characters
last_5 = text[-5:]  # ✓ 'World'

# Get middle portion
middle = text[6:11]  # ✓ 'World' (or text[6:])

# Remove first N characters
without_first_6 = text[6:]  # ✓ 'World'

# Remove last N characters
without_last_6 = text[:-6]  # ✓ 'Hello'

# Get every 2nd character
every_2nd = text[::2]  # ✓ 'HloWrd'

# Reverse string
reversed_text = text[::-1]  # ✓ 'dlroW olleH'

# Safe slicing (never errors)
substring = text[100:200]  # ✓ '' (empty, no error)

# Get substring between positions
start = 0
end = 5
substring = text[start:end]  # ✓ 'Hello'

# Extract file extension
filename = "document.txt"
extension = filename[filename.rfind('.'):]  # ✓ '.txt'
# Or better:
extension = filename.split('.')[-1]  # ✓ 'txt'
```

**Slicing Patterns:**
```python
text = "Python Programming"

# First word
first_word = text.split()[0]  # 'Python'
# Or: text[:text.find(' ')]

# Last word
last_word = text.split()[-1]  # 'Programming'
# Or: text[text.rfind(' ')+1:]

# First N characters
text[:5]  # 'Pytho'

# Last N characters  
text[-5:]  # 'mming'

# Middle portion
text[7:18]  # 'Programming'

# Remove whitespace from ends
text.strip()  # Removes leading/trailing spaces

# Reverse
text[::-1]  # 'gnimmargorP nohtyP'
```

---

## 3.3 String Methods

### Common String Methods

```python
text = "Hello World"

# Case conversion
text.upper()        # 'HELLO WORLD'
text.lower()        # 'hello world'
text.capitalize()   # 'Hello world'
text.title()        # 'Hello World'

# Checking content
text.startswith('Hello')  # True
text.endswith('World')    # True
text.isalpha()      # False (has space)
text.isdigit()      # False
text.isalnum()      # False (has space)

# Finding substrings
text.find('World')   # 6 (index where found)
text.find('Python')  # -1 (not found)
text.index('World')  # 6 (raises ValueError if not found)

# Replacing
text.replace('World', 'Python')  # 'Hello Python'

# Splitting and joining
words = text.split()  # ['Hello', 'World']
joined = ' '.join(words)  # 'Hello World'

# Stripping whitespace
"  hello  ".strip()   # 'hello'
"  hello  ".lstrip()  # 'hello  '
"  hello  ".rstrip()  # '  hello'
```

---

### Error Type 3: `AttributeError: 'str' object has no attribute 'X'`

**Error Message:**
```python
>>> text = "hello"
>>> text.append("!")
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: 'str' object has no attribute 'append'
```

**What Happened:**
You tried to use a method that doesn't exist for strings, or you made a typo.

**Why It Happens:**
- Using list methods on strings
- Typo in method name
- Confusing similar methods
- Wrong object type

**Code Example - WRONG:**
```python
text = "hello"

# List methods don't work on strings
text.append("!")  # ERROR! Strings don't have append

# Typo in method name
text.uppper()  # ERROR! Typo: should be upper()

# Wrong method
text.add("world")  # ERROR! No 'add' method

# Trying to modify immutably
text[0] = 'H'  # ERROR! Strings are immutable

# Wrong assumptions
text.remove('l')  # ERROR! Strings don't have remove
```

**Code Example - CORRECT:**
```python
text = "hello"

# Use string concatenation instead of append
text = text + "!"  # ✓ "hello!"
# Or use +=
text += "!"  # ✓

# Correct method names
text.upper()  # ✓ "HELLO"
text.lower()  # ✓ "hello"

# Use replace to "modify" strings
text = text.replace('h', 'H')  # ✓ "Hello"

# Remove characters with replace
text = text.replace('l', '')  # ✓ "heo" (removes all 'l')

# Create new string instead of modifying
text = "hello"
new_text = 'H' + text[1:]  # ✓ "Hello"

# Check if method exists
if hasattr(text, 'upper'):
    result = text.upper()  # ✓

# Common string methods (not list methods)
text = "hello world"
text.split()          # ✓ Returns list: ['hello', 'world']
text.replace('o', '0') # ✓ Returns: 'hell0 w0rld'
text.find('world')    # ✓ Returns: 6
text.startswith('h')  # ✓ Returns: True
text.strip()          # ✓ Removes whitespace
text.count('l')       # ✓ Returns: 3
```

**String Method Reference:**
```python
text = "Hello World"

# Case methods
text.upper()      # HELLO WORLD
text.lower()      # hello world
text.capitalize() # Hello world
text.title()      # Hello World
text.swapcase()   # hELLO wORLD

# Checking methods (return bool)
text.isalpha()    # False (has space)
text.isdigit()    # False
text.isalnum()    # False
text.isspace()    # False
text.isupper()    # False
text.islower()    # False
text.istitle()    # True

# Search methods
text.find('o')        # 4 (first occurrence)
text.rfind('o')       # 7 (last occurrence)
text.index('o')       # 4 (like find, but raises ValueError if not found)
text.count('o')       # 2
text.startswith('He') # True
text.endswith('ld')   # True

# Modification methods (return new string)
text.replace('World', 'Python')  # Hello Python
text.strip()          # Removes whitespace
text.lstrip()         # Removes left whitespace
text.rstrip()         # Removes right whitespace

# Split and join
text.split()          # ['Hello', 'World']
text.split('o')       # ['Hell', ' W', 'rld']
' '.join(['a', 'b'])  # 'a b'

# Padding and alignment
text.center(20)       # '    Hello World     '
text.ljust(20)        # 'Hello World         '
text.rjust(20)        # '         Hello World'
text.zfill(20)        # '000000000Hello World'
```

---

## 3.4 String Formatting

### String Formatting Methods

```python
# f-strings (Python 3.6+) - RECOMMENDED
name = "Alice"
age = 25
message = f"My name is {name} and I'm {age} years old"
# "My name is Alice and I'm 25 years old"

# format() method
message = "My name is {} and I'm {} years old".format(name, age)

# %-formatting (old style)
message = "My name is %s and I'm %d years old" % (name, age)

# Formatting numbers
pi = 3.14159
formatted = f"Pi is {pi:.2f}"  # "Pi is 3.14"

# Formatting with width
number = 42
formatted = f"Number: {number:5d}"  # "Number:    42"
```

---

### Error Type 4: `ValueError: invalid format string`

**Error Message:**
```python
>>> name = "Alice"
>>> message = f"Hello {nme}"  # Typo in variable name
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
NameError: name 'nme' is not defined
```

**What Happened:**
Errors in f-strings or format strings.

**Code Example - WRONG:**
```python
# Typo in variable name
name = "Alice"
message = f"Hello {nme}"  # ERROR! 'nme' not defined

# Wrong number of arguments in format()
template = "Hello {} and {}"
message = template.format("Alice")  # ERROR! Needs 2 arguments

# Wrong format specifier
number = 42
formatted = f"{number:.2f}"  # ERROR! Can't format int as float
# (Actually works in Python 3, but conceptually wrong)

# Unclosed brace
message = f"Hello {name"  # ERROR! SyntaxError: unclosed '{'

# Wrong %-formatting
name = "Alice"
age = 25
message = "Name: %s, Age: %s" % name  # ERROR! Needs tuple with 2 items
```

**Code Example - CORRECT:**
```python
# Correct variable name
name = "Alice"
message = f"Hello {name}"  # ✓

# Correct number of arguments
template = "Hello {} and {}"
message = template.format("Alice", "Bob")  # ✓

# Correct format specifier for float
number = 42.5
formatted = f"{number:.2f}"  # ✓ "42.50"

# Convert int to float if needed
number = 42
formatted = f"{float(number):.2f}"  # ✓ "42.00"

# Closed braces
message = f"Hello {name}"  # ✓

# Correct %-formatting
name = "Alice"
age = 25
message = "Name: %s, Age: %d" % (name, age)  # ✓ Tuple

# f-string with expressions
x = 10
y = 20
message = f"Sum is {x + y}"  # ✓ "Sum is 30"

# Format with padding
number = 42
formatted = f"{number:05d}"  # ✓ "00042" (5 digits, zero-padded)

# Multiple variables
first = "Alice"
last = "Smith"
age = 25
message = f"{first} {last} is {age} years old"  # ✓

# Format numbers in f-strings
price = 19.99
message = f"Price: ${price:.2f}"  # ✓ "Price: $19.99"

# Use format() safely
message = "Hello {}".format(name)  # ✓
message = "Hello {name}".format(name=name)  # ✓ Named

# Escape braces
message = f"Use {{braces}} in f-strings"  # ✓ "Use {braces} in f-strings"
```

**Format Specifier Reference:**
```python
number = 42
pi = 3.14159
text = "hello"

# Integer formatting
f"{number:d}"     # '42' (decimal)
f"{number:5d}"    # '   42' (width 5)
f"{number:05d}"   # '00042' (zero-padded)

# Float formatting
f"{pi:f}"         # '3.141590' (default 6 decimals)
f"{pi:.2f}"       # '3.14' (2 decimals)
f"{pi:8.2f}"      # '    3.14' (width 8, 2 decimals)

# String formatting
f"{text:s}"       # 'hello'
f"{text:>10s}"    # '     hello' (right-aligned, width 10)
f"{text:<10s}"    # 'hello     ' (left-aligned)
f"{text:^10s}"    # '  hello   ' (centered)

# Percentage
value = 0.85
f"{value:.1%}"    # '85.0%'

# Scientific notation
large = 1000000
f"{large:e}"      # '1.000000e+06'
f"{large:.2e}"    # '1.00e+06'
```

---

## 3.5 String Immutability

### Understanding Immutability

```python
# Strings are immutable - cannot be changed
text = "hello"

# This creates a NEW string
text = text.upper()  # "HELLO"

# Original string unchanged (if referenced elsewhere)
original = "hello"
modified = original.upper()
print(original)  # Still "hello"
print(modified)  # "HELLO"
```

---

### Error Type 5: `TypeError: 'str' object does not support item assignment`

**Error Message:**
```python
>>> text = "hello"
>>> text[0] = 'H'
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: 'str' object does not support item assignment
```

**What Happened:**
You tried to change a character in a string. Strings are immutable in Python.

**Why It Happens:**
- Trying to modify string directly
- Treating string like a list
- Not understanding immutability

**Code Example - WRONG:**
```python
text = "hello"

# Can't modify characters
text[0] = 'H'  # ERROR! Strings are immutable

# Can't delete characters
del text[0]  # ERROR! Can't delete from immutable

# Can't use list methods that modify
text.append('!')  # ERROR! No such method
text.remove('l')  # ERROR! No such method
text.insert(0, 'H')  # ERROR! No such method
```

**Code Example - CORRECT:**
```python
text = "hello"

# Create new string with change
text = 'H' + text[1:]  # ✓ "Hello"

# Use string methods that return new strings
text = text.replace('h', 'H')  # ✓ "Hello"

# Build new string
text = "hello"
new_text = ""
for char in text:
    if char == 'h':
        new_text += 'H'
    else:
        new_text += char
# new_text is "Hello"

# Use list for modifications, then join
text = "hello"
chars = list(text)  # ['h', 'e', 'l', 'l', 'o']
chars[0] = 'H'      # ✓ Lists are mutable
text = ''.join(chars)  # ✓ "Hello"

# Multiple replacements
text = "hello world"
text = text.replace('h', 'H').replace('w', 'W')
# "Hello World"

# Remove characters
text = "hello"
text = text.replace('l', '')  # ✓ "heo"

# Insert characters (create new string)
text = "heo"
text = text[:2] + 'll' + text[2:]  # ✓ "hello"

# Reverse string (creates new)
text = "hello"
reversed_text = text[::-1]  # ✓ "olleh"
```

**Why Immutability Matters:**
```python
# Immutability makes strings safe
def process_text(text):
    text = text.upper()  # Creates NEW string
    return text

original = "hello"
result = process_text(original)
print(original)  # Still "hello" - unchanged
print(result)    # "HELLO" - new string

# Multiple references to same string
text1 = "hello"
text2 = text1
text1 = text1.upper()  # Creates NEW string
print(text1)  # "HELLO"
print(text2)  # "hello" - unchanged

# This is different from lists (mutable)
list1 = [1, 2, 3]
list2 = list1
list1.append(4)  # Modifies same list
print(list1)  # [1, 2, 3, 4]
print(list2)  # [1, 2, 3, 4] - also changed!
```

---

## 3.6 Common String Operations

### Checking String Content

```python
text = "Hello123"

# Check if all characters are letters
text.isalpha()  # False (has digits)

# Check if all characters are digits
text.isdigit()  # False (has letters)

# Check if alphanumeric (letters and digits)
text.isalnum()  # True

# Check if all lowercase
"hello".islower()  # True
"Hello".islower()  # False

# Check if all uppercase
"HELLO".isupper()  # True

# Check if empty
text = ""
if text:  # False - empty string is falsy
    print("Has content")
else:
    print("Empty")

# Check if whitespace only
"   ".isspace()  # True
"hello".isspace()  # False
```

---

### Error Type 6: String Comparison Pitfalls

**Code Example - WRONG LOGIC:**
```python
# Case-sensitive comparison
name = "Alice"
if name == "alice":  # False!
    print("Match")

# Leading/trailing whitespace
text = "  hello  "
if text == "hello":  # False!
    print("Match")

# Type mismatch
number = "5"
if number == 5:  # False! String vs int
    print("Match")

# Empty string checks
text = ""
if text == None:  # False! Empty string is not None
    print("None")
```

**Code Example - CORRECT:**
```python
# Case-insensitive comparison
name = "Alice"
if name.lower() == "alice":  # ✓ True
    print("Match")

# Strip whitespace first
text = "  hello  "
if text.strip() == "hello":  # ✓ True
    print("Match")

# Convert types before comparing
number = "5"
if int(number) == 5:  # ✓ True
    print("Match")

# Or convert the other way
if number == str(5):  # ✓ True
    print("Match")

# Check for empty string
text = ""
if not text:  # ✓ True - empty string is falsy
    print("Empty")

# Or explicitly
if text == "":  # ✓ True
    print("Empty")

# Check for None
value = None
if value is None:  # ✓ True
    print("None")

# Check for empty or None
if not text or text is None:  # ✓
    print("Empty or None")

# Substring checking
text = "Hello World"
if "World" in text:  # ✓ True
    print("Contains 'World'")

# Starts with / ends with
filename = "document.txt"
if filename.endswith('.txt'):  # ✓ True
    print("Text file")
```

---

## 3.7 Practice Problems - Fix These Errors!

### Problem 1: Index Out of Range
```python
text = "Python"
print(text[6])
```

<details>
<summary>Click for Answer</summary>

**Error:** `IndexError: string index out of range`

**Why:** String has indices 0-5, trying to access index 6

**Fix:**
```python
text = "Python"
print(text[5])  # ✓ Last character: 'n'
# Or
print(text[-1])  # ✓ Better: 'n'
```
</details>

---

### Problem 2: Wrong Method
```python
text = "hello"
text.append(" world")
```

<details>
<summary>Click for Answer</summary>

**Error:** `AttributeError: 'str' object has no attribute 'append'`

**Why:** Strings don't have append method (lists do)

**Fix:**
```python
text = "hello"
text = text + " world"  # ✓ Concatenation
# Or
text += " world"  # ✓ Also works
print(text)  # "hello world"
```
</details>

---

### Problem 3: String Immutability
```python
text = "hello"
text[0] = 'H'
```

<details>
<summary>Click for Answer</summary>

**Error:** `TypeError: 'str' object does not support item assignment`

**Why:** Strings are immutable

**Fix:**
```python
text = "hello"
text = 'H' + text[1:]  # ✓ Create new string
print(text)  # "Hello"

# Or use replace
text = "hello"
text = text.replace('h', 'H')  # ✓ "Hello"
```
</details>

---

### Problem 4: Format String Error
```python
name = "Alice"
age = 25
message = f"Hello {name}, you are {ag} years old"
```

<details>
<summary>Click for Answer</summary>

**Error:** `NameError: name 'ag' is not defined`

**Why:** Typo in variable name inside f-string

**Fix:**
```python
name = "Alice"
age = 25
message = f"Hello {name}, you are {age} years old"  # ✓ Correct variable name
print(message)  # "Hello Alice, you are 25 years old"
```
</details>

---

### Problem 5: Comparison Error
```python
text = "  hello  "
if text == "hello":
    print("Match")
else:
    print("No match")
```

<details>
<summary>Click for Answer</summary>

**Issue:** Prints "No match" due to whitespace

**Fix:**
```python
text = "  hello  "
if text.strip() == "hello":  # ✓ Strip whitespace first
    print("Match")
else:
    print("No match")
# Prints: "Match"
```
</details>

---

## 3.8 Key Takeaways

### What You Learned

1. **Check string length before indexing** - Avoid IndexError
2. **Use correct string methods** - Strings don't have list methods
3. **Strings are immutable** - Create new strings, don't modify
4. **Use f-strings for formatting** - Modern and clear
5. **Strip whitespace when comparing** - Avoid comparison issues
6. **Case-insensitive comparison** - Use .lower() or .upper()
7. **Slicing never errors** - But check logic for correct results

### Common Patterns

```python
# Pattern 1: Safe character access
if len(text) > index:
    char = text[index]

# Pattern 2: Case-insensitive comparison
if text.lower() == "hello":
    print("Match")

# Pattern 3: Clean and compare
if text.strip() == "expected":
    print("Match")

# Pattern 4: Create new string from old
text = 'H' + text[1:]

# Pattern 5: Check empty string
if not text:
    print("Empty")

# Pattern 6: Safe substring check
if "substring" in text:
    print("Found")
```

### Error Summary Table

| Error Type | Common Cause | Prevention |
|------------|--------------|------------|
| `IndexError` | Index beyond string length | Check len() first |
| `AttributeError` | Wrong method or typo | Use correct string methods |
| `TypeError` | Trying to modify string | Create new string instead |
| `ValueError` | Wrong format string | Check variable names and format |

---

## 3.9 Moving Forward

You now understand strings and string methods. You can:
- Access characters safely
- Use string methods correctly
- Format strings properly
- Handle immutability
- Compare strings accurately

In **Chapter 4**, we'll explore **Lists and List Methods** - working with collections and avoiding list errors!
