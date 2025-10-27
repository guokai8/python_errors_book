# Chapter 7: Conditional Statements - Logic and Flow Control Errors

## Introduction

You've mastered data structures. Now let's explore **conditional statements** - the foundation of program logic. Conditionals let your code make decisions and execute different code based on conditions.

Common errors:
- **IndentationError**: Wrong indentation in Python
- **SyntaxError**: Wrong syntax in if/elif/else
- **NameError**: Variables not defined
- Logic errors (wrong conditions)

Conditionals control program flow. Let's master them!

---

## 7.1 If Statement Basics

### Simple If Statements

```python
# Basic if statement
age = 18
if age >= 18:
    print("Adult")

# If with else
age = 15
if age >= 18:
    print("Adult")
else:
    print("Minor")

# If with elif
score = 85
if score >= 90:
    print("A")
elif score >= 80:
    print("B")
elif score >= 70:
    print("C")
else:
    print("F")

# Multiple conditions
age = 25
has_license = True
if age >= 18 and has_license:
    print("Can drive")
```

---

### Error Type 1: `IndentationError: expected an indented block`

**Error Message:**
```python
>>> if True:
... print("Hello")
  File "<stdin>", line 2
    print("Hello")
    ^
IndentationError: expected an indented block
```

**What Happened:**
Python requires indentation after if statements. The code block must be indented.

**Why It Happens:**
- Missing indentation
- Inconsistent indentation (tabs vs spaces)
- Wrong indentation level
- Copy-paste errors

**Code Example - WRONG:**
```python
# No indentation
if True:
print("Hello")  # ERROR! Must be indented

# Inconsistent indentation
if True:
    print("Line 1")  # 4 spaces
  print("Line 2")    # ERROR! 2 spaces

# Tab and space mixing
if True:
	print("Tab")    # Tab character
    print("Spaces")  # ERROR! Spaces (looks same but different)

# Wrong else indentation
if True:
    print("If block")
  else:  # ERROR! else should align with if
    print("Else block")

# Empty if block
if True:
# ERROR! Need pass or code

# Multiple levels wrong
if True:
    if True:
    print("Wrong")  # ERROR! Should be indented more
```

**Code Example - CORRECT:**
```python
# Proper indentation (4 spaces is standard)
if True:
    print("Hello")  # ✓ Indented 4 spaces

# Consistent indentation
if True:
    print("Line 1")  # 4 spaces
    print("Line 2")  # 4 spaces ✓

# Correct else alignment
if True:
    print("If block")
else:  # ✓ Aligned with if
    print("Else block")

# Empty if block - use pass
if True:
    pass  # ✓ Placeholder

# Multiple levels
if True:
    print("Level 1")
    if True:
        print("Level 2")  # ✓ Indented further

# Elif alignment
if score >= 90:
    print("A")
elif score >= 80:  # ✓ Aligned with if
    print("B")
else:  # ✓ Aligned with if
    print("F")

# Configure editor:
# - Use spaces, not tabs
# - Set indent to 4 spaces
# - Enable "show whitespace" to see issues
```

**Python Indentation Rules:**
```python
# Standard is 4 spaces per level
if condition:
    # Level 1 (4 spaces)
    if nested_condition:
        # Level 2 (8 spaces)
        print("Nested")
    print("Level 1")

# All lines in same block must have same indentation
if True:
    print("Line 1")  # 4 spaces
    print("Line 2")  # 4 spaces ✓
    # print("Line 3")  # Comment - indentation doesn't matter

# Control structures need indentation
if True:
    print("If")
else:
    print("Else")

for i in range(3):
    print(i)

while True:
    break

def function():
    return

class MyClass:
    pass
```

---

## 7.2 Comparison Operators

### Using Comparisons Correctly

```python
# Comparison operators
x = 10

# Equal
x == 10  # True

# Not equal
x != 5   # True

# Greater/less than
x > 5    # True
x < 20   # True
x >= 10  # True
x <= 10  # True

# Chaining comparisons
5 < x < 15  # True (same as: 5 < x and x < 15)
```

---

### Error Type 2: `SyntaxError: invalid syntax` (in conditions)

**Error Message:**
```python
>>> if x = 10:
  File "<stdin>", line 1
    if x = 10:
         ^
SyntaxError: invalid syntax
```

**What Happened:**
Using assignment (=) instead of comparison (==) in condition.

**Why It Happens:**
- Confusing = and ==
- Missing colon after condition
- Wrong operator
- Incomplete condition

**Code Example - WRONG:**
```python
x = 10

# Assignment instead of comparison
if x = 10:  # ERROR! Use ==, not =
    print("Ten")

# Missing colon
if x == 10  # ERROR! Missing :
    print("Ten")

# Wrong syntax
if x == 10 and:  # ERROR! Incomplete condition
    print("Ten")

# Chaining without variable
if 5 < x < and x < 15:  # ERROR! Incomplete
    print("Range")

# Using 'is' for value comparison
if x is 10:  # Wrong! Use == for values
    print("Ten")  # May not work as expected

# Missing condition entirely
if:  # ERROR! Need condition
    print("Hello")
```

**Code Example - CORRECT:**
```python
x = 10

# Use == for comparison
if x == 10:  # ✓ Comparison
    print("Ten")

# Include colon
if x == 10:  # ✓ Colon required
    print("Ten")

# Complete conditions
if x == 10 and x > 0:  # ✓ Complete
    print("Ten and positive")

# Proper chaining
if 5 < x < 15:  # ✓
    print("In range")

# Or explicit
if x > 5 and x < 15:  # ✓ Also works
    print("In range")

# Use == for value comparison
if x == 10:  # ✓ Correct for values
    print("Ten")

# Use 'is' only for None, True, False
if x is None:  # ✓ Correct for None
    print("None")

if x is True:  # ✓ Correct for True/False
    print("True")

# Parentheses for clarity (optional but helpful)
if (x > 5) and (x < 15):  # ✓ Very clear
    print("In range")

# Multiple conditions
if x > 0 and x < 100 and x % 2 == 0:  # ✓
    print("Even number between 0 and 100")
```

**Comparison Operators Reference:**
```python
# All comparison operators:
==  # Equal to
!=  # Not equal to
<   # Less than
>   # Greater than
<=  # Less than or equal to
>=  # Greater than or equal to

# Identity operators:
is      # Same object (use for None, True, False)
is not  # Different object

# Membership operators:
in      # Item in collection
not in  # Item not in collection

# Examples:
x == 5          # Value comparison
x is None       # Identity comparison
"a" in "apple"  # Membership test
x not in [1,2]  # Negative membership
```

---

## 7.3 Logical Operators

### Combining Conditions

```python
# AND - both must be True
if age >= 18 and has_license:
    print("Can drive")

# OR - at least one must be True
if is_weekend or is_holiday:
    print("Day off")

# NOT - reverses boolean
if not is_raining:
    print("Go outside")

# Complex combinations
if (age >= 18 and has_license) or is_instructor:
    print("Can drive")

# Short-circuit evaluation
if user is not None and user.is_active():
    # user.is_active() only called if user is not None
    print("Active user")
```

---

### Error Type 3: Logic Errors (No Python Error)

**What Happened:**
Code runs but produces wrong results due to incorrect logic.

**Code Example - WRONG LOGIC:**
```python
# Wrong order of checks
score = 85
if score >= 70:
    print("C")  # Prints "C" even though should be "B"
elif score >= 80:
    print("B")  # Never reached!
elif score >= 90:
    print("A")  # Never reached!

# Wrong operator
age = 25
if age > 18:  # Should be >=
    print("Adult")  # Excludes 18-year-olds

# Missing parentheses
if x > 5 and y > 3 or z > 10:
    # Evaluated as: (x > 5 and y > 3) or z > 10
    # Might not be what you want!
    print("Condition met")

# Using 'is' instead of ==
x = 1000
y = 1000
if x is y:  # False (different objects)
    print("Same")  # Doesn't print (wrong!)

# Comparing strings case-sensitively
name = "Alice"
if name == "alice":  # False due to case
    print("Match")  # Doesn't print
```

**Code Example - CORRECT:**
```python
# Correct order (most specific first)
score = 85
if score >= 90:
    print("A")
elif score >= 80:
    print("B")  # ✓ Prints correctly
elif score >= 70:
    print("C")
else:
    print("F")

# Correct operator (>= includes 18)
age = 18
if age >= 18:  # ✓ Includes 18
    print("Adult")

# Use parentheses for clarity
if (x > 5 and y > 3) or z > 10:  # ✓ Clear intent
    print("Condition met")

# Use == for value comparison
x = 1000
y = 1000
if x == y:  # ✓ True (same value)
    print("Same")

# Case-insensitive string comparison
name = "Alice"
if name.lower() == "alice":  # ✓ True
    print("Match")

# Handle None safely
value = None
if value is None:  # ✓ Correct
    print("No value")

# Check type and value
if isinstance(x, int) and x > 0:  # ✓ Safe
    print("Positive integer")

# Multiple conditions with clear logic
age = 25
has_license = True
has_insurance = True
if age >= 18:
    if has_license and has_insurance:
        print("Can drive")  # ✓ Clear logic
    else:
        print("Need license or insurance")
else:
    print("Too young")
```

---

## 7.4 Truthiness and Falsiness

### Understanding Boolean Context

```python
# Falsy values (evaluate to False)
if False:        pass  # False
if None:         pass  # None
if 0:            pass  # Zero
if 0.0:          pass  # Zero float
if "":           pass  # Empty string
if []:           pass  # Empty list
if {}:           pass  # Empty dict
if set():        pass  # Empty set
if ():           pass  # Empty tuple

# Truthy values (everything else)
if True:         pass  # ✓ True
if 1:            pass  # ✓ Non-zero number
if "text":       pass  # ✓ Non-empty string
if [1]:          pass  # ✓ Non-empty list
if {"a": 1}:     pass  # ✓ Non-empty dict

# Common patterns
# Check if list has items
items = [1, 2, 3]
if items:  # ✓ True if not empty
    print("Has items")

# Check if string is not empty
text = "hello"
if text:  # ✓ True if not empty
    print("Has text")

# Check if variable is not None
value = 10
if value is not None:  # ✓ Explicit None check
    print("Has value")

# Be careful with zero
count = 0
if count:  # False! 0 is falsy
    print("Has count")  # Won't print

# Better:
if count is not None:  # ✓ True even if count is 0
    print("Has count")
```

---

### Error Type 4: Truthiness Pitfalls

**Code Example - WRONG LOGIC:**
```python
# Treating zero as "no value"
count = 0
if count:  # False - treats 0 as falsy
    print(f"Count: {count}")  # Doesn't print (wrong!)

# Confusing empty string with None
text = ""
if text:  # False - empty string is falsy
    print(text)  # Doesn't print
else:
    text = "Default"  # Sets default even if string was intentionally empty

# Not checking type
value = []
if value:  # False - empty list is falsy
    print("Has value")  # Doesn't print even though list exists

# Using boolean literal comparison
is_valid = True
if is_valid == True:  # Works but verbose
    print("Valid")
```

**Code Example - CORRECT:**
```python
# Check for None explicitly when zero is valid
count = 0
if count is not None:  # ✓ True even if count is 0
    print(f"Count: {count}")  # Prints "Count: 0"

# Or check the specific range you want
if count >= 0:  # ✓ Explicitly check non-negative
    print(f"Count: {count}")

# Distinguish empty string from None
text = ""
if text is not None:  # ✓ True even if empty string
    print(f"Text: '{text}'")  # Prints "Text: ''"

# Check type and content separately
value = []
if value is not None:  # ✓ List exists
    if value:  # ✓ List has items
        print("Has items")
    else:
        print("Empty list")  # ✓

# Direct boolean check (more Pythonic)
is_valid = True
if is_valid:  # ✓ Cleaner
    print("Valid")

# Check length explicitly
items = []
if len(items) > 0:  # ✓ Explicit check
    print("Has items")

# Or direct
if items:  # ✓ Also works for non-empty check
    print("Has items")
```

---

## 7.5 Ternary Operator

### Conditional Expressions

```python
# Traditional if/else
if x > 0:
    result = "positive"
else:
    result = "non-positive"

# Ternary operator (conditional expression)
result = "positive" if x > 0 else "non-positive"

# More examples
status = "adult" if age >= 18 else "minor"

max_value = a if a > b else b

message = "Even" if x % 2 == 0 else "Odd"

# Nested ternary (avoid if too complex)
grade = "A" if score >= 90 else "B" if score >= 80 else "C"
```

---

### Error Type 5: Ternary Syntax Errors

**Code Example - WRONG:**
```python
# Wrong order
result = if x > 0 "positive" else "negative"  # ERROR! Syntax wrong

# Missing parts
result = "positive" if x > 0  # ERROR! Missing else

# Wrong keyword
result = "positive" when x > 0 else "negative"  # ERROR! Use 'if'

# Too complex (hard to read)
result = "A" if score >= 90 else "B" if score >= 80 else "C" if score >= 70 else "D" if score >= 60 else "F"
# Technically works but hard to read!
```

**Code Example - CORRECT:**
```python
# Correct ternary syntax
result = "positive" if x > 0 else "negative"  # ✓

# With else (required)
result = "positive" if x > 0 else "non-positive"  # ✓

# Correct keyword (if, not when)
result = "positive" if x > 0 else "negative"  # ✓

# Complex logic - use regular if/else instead
if score >= 90:
    grade = "A"
elif score >= 80:
    grade = "B"
elif score >= 70:
    grade = "C"
elif score >= 60:
    grade = "D"
else:
    grade = "F"
# ✓ Much more readable!

# Good ternary uses (simple, readable)
status = "online" if is_connected else "offline"  # ✓
sign = "+" if x >= 0 else "-"  # ✓
color = "red" if temp > 30 else "blue"  # ✓

# Ternary with function calls
result = process_data() if data else default_value()  # ✓
```

---

## 7.6 Common Conditional Patterns

### Useful Patterns

```python
# Check multiple conditions
if x and y and z:
    print("All true")

# Check any condition
if x or y or z:
    print("At least one true")

# Range checking
if 0 <= score <= 100:
    print("Valid score")

# Membership testing
if item in collection:
    print("Found")

# Type checking
if isinstance(value, int):
    print("Integer")

# None checking
if value is None:
    print("No value")

# Empty checking
if not items:
    print("Empty")

# Combining checks
if value is not None and value > 0:
    print("Positive value")

# Guard clauses (early return)
def process(data):
    if not data:
        return  # Exit early
    if not valid(data):
        return  # Exit early
    # Main logic here
    return result
```

---

## 7.7 Practice Problems - Fix These Errors!

### Problem 1: Indentation Error
```python
if True:
print("Hello")
```

<details>
<summary>Click for Answer</summary>

**Error:** `IndentationError: expected an indented block`

**Why:** Code after if must be indented

**Fix:**
```python
if True:
    print("Hello")  # ✓ Indented 4 spaces
```
</details>

---

### Problem 2: Assignment in Condition
```python
x = 10
if x = 10:
    print("Ten")
```

<details>
<summary>Click for Answer</summary>

**Error:** `SyntaxError: invalid syntax`

**Why:** Using = (assignment) instead of == (comparison)

**Fix:**
```python
x = 10
if x == 10:  # ✓ Use == for comparison
    print("Ten")
```
</details>

---

### Problem 3: Wrong Logic Order
```python
score = 85
if score >= 70:
    grade = "C"
elif score >= 80:
    grade = "B"
elif score >= 90:
    grade = "A"
print(grade)
```

<details>
<summary>Click for Answer</summary>

**Issue:** Prints "C" instead of "B" (logic error)

**Why:** Checks are in wrong order (70 before 80)

**Fix:**
```python
score = 85
# Check highest first
if score >= 90:
    grade = "A"
elif score >= 80:
    grade = "B"  # ✓ Now this is checked
elif score >= 70:
    grade = "C"
else:
    grade = "F"
print(grade)  # Prints "B"
```
</details>

---

### Problem 4: Truthiness Error
```python
count = 0
if count:
    print(f"Count is {count}")
else:
    print("No count")
```

<details>
<summary>Click for Answer</summary>

**Issue:** Prints "No count" even though count is 0 (a valid value)

**Why:** 0 is falsy in Python

**Fix:**
```python
count = 0
if count is not None:  # ✓ Check for None explicitly
    print(f"Count is {count}")  # Prints "Count is 0"
else:
    print("No count")

# Or check specific range
if count >= 0:  # ✓ Check for non-negative
    print(f"Count is {count}")
```
</details>

---

### Problem 5: Missing Colon
```python
if x > 5
    print("Greater than 5")
```

<details>
<summary>Click for Answer</summary>

**Error:** `SyntaxError: invalid syntax`

**Why:** Missing colon after condition

**Fix:**
```python
if x > 5:  # ✓ Add colon
    print("Greater than 5")
```
</details>

---

## 7.8 Key Takeaways

### What You Learned

1. **Indent code blocks** - 4 spaces after if/elif/else
2. **Use == for comparison** - Not = (assignment)
3. **Order matters** - Check most specific conditions first
4. **Use 'is' for None** - Not == for None checks
5. **Understand truthiness** - Empty collections are falsy
6. **Include colon** - After every condition
7. **Use parentheses** - For complex conditions

### Common Patterns

```python
# Pattern 1: Range check
if 0 <= value <= 100:
    pass

# Pattern 2: None check
if value is not None:
    pass

# Pattern 3: Empty check
if not items:
    pass

# Pattern 4: Safe chaining
if user and user.is_active():
    pass

# Pattern 5: Multiple conditions
if condition1 and condition2:
    pass
```

### Error Summary Table

| Error Type | Common Cause | Prevention |
|------------|--------------|------------|
| `IndentationError` | Missing/wrong indentation | Use 4 spaces consistently |
| `SyntaxError` | Using = instead of == | Use == for comparison |
| `SyntaxError` | Missing colon | Add : after condition |
| Logic errors | Wrong order/operators | Check most specific first |

---

## 7.9 Moving Forward

You now understand conditional statements. You can:
- Write if/elif/else correctly
- Use proper indentation
- Compare values accurately
- Handle None and empty values
- Write clear logic

In **Chapter 8**, we'll explore **Loops** - for and while loops for iteration!

