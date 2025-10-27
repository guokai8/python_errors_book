
# Wrong escape
pattern = '\d+'  # Should be raw string
re.search(pattern, text)  # Might not work as expected
```

**Code Example - CORRECT:**
```python
import re

# Close all parentheses
pattern = r'(hello)'  # ✓ Closed
match = re.search(pattern, 'hello world')

# Close all brackets
pattern = r'[abc]'  # ✓ Closed
match = re.search(pattern, 'abc')

# Valid quantifier syntax
pattern = r'a{0,5}'  # ✓ Valid: 0 to 5 times
pattern = r'a{5}'    # ✓ Valid: exactly 5 times
pattern = r'a{5,}'   # ✓ Valid: 5 or more times

# Escape special characters
pattern = r'price: \$50'  # ✓ Escaped $
match = re.search(pattern, 'price: $50')

# Use raw strings for regex patterns
pattern = r'\d+'  # ✓ Raw string (r prefix)
match = re.search(pattern, '123')

# Test pattern before using
try:
    re.compile(pattern)
except re.error as e:
    print(f"Invalid pattern: {e}")
```

**Regex Special Characters:**
```python
# Characters that need escaping:
. ^ $ * + ? { } [ ] \ | ( )

# To match literally, escape with \
pattern = r'\.'   # Matches literal .
pattern = r'\$'   # Matches literal $
pattern = r'\('   # Matches literal (
pattern = r'\\'   # Matches literal \

# In character class, different rules
pattern = r'[.]'  # . doesn't need escape in []
pattern = r'[\^]' # ^ needs escape in []
```

---

## 11.2 Common Regex Patterns

### Useful Patterns

```python
import re

# Digits
pattern = r'\d'     # Any digit [0-9]
pattern = r'\d+'    # One or more digits
pattern = r'\d{3}'  # Exactly 3 digits

# Letters
pattern = r'[a-z]'  # Lowercase letter
pattern = r'[A-Z]'  # Uppercase letter
pattern = r'[a-zA-Z]'  # Any letter

# Whitespace
pattern = r'\s'     # Any whitespace
pattern = r'\s+'    # One or more whitespace

# Word characters
pattern = r'\w'     # Letter, digit, or underscore
pattern = r'\w+'    # One or more word characters

# Beginning and end
pattern = r'^hello' # Starts with "hello"
pattern = r'world$' # Ends with "world"

# Email pattern
email_pattern = r'[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}'

# Phone pattern (US)
phone_pattern = r'\d{3}-\d{3}-\d{4}'

# URL pattern
url_pattern = r'https?://[^\s]+'
```

---

## 11.3 Match vs Search vs Findall

### Understanding Different Methods

```python
import re

text = "The cat and the bat sat on the mat"

# match() - checks beginning only
match = re.match(r'cat', text)
print(match)  # None - doesn't start with 'cat'

match = re.match(r'The', text)
print(match)  # <Match object> - starts with 'The'

# search() - finds first occurrence anywhere
match = re.search(r'cat', text)
print(match.group())  # 'cat' - found it

# findall() - finds all occurrences
matches = re.findall(r'at', text)
print(matches)  # ['at', 'at', 'at', 'at']

# finditer() - iterator of match objects
for match in re.finditer(r'at', text):
    print(f"Found at position {match.start()}: {match.group()}")
```

---

### Error Type 2: `AttributeError: 'NoneType' object has no attribute 'group'`

**Error Message:**
```python
>>> import re
>>> match = re.search(r'xyz', 'abc')
>>> print(match.group())
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: 'NoneType' object has no attribute 'group'
```

**What Happened:**
Pattern not found, match is None, trying to call .group().

**Why It Happens:**
- Pattern doesn't exist in text
- Wrong pattern
- Not checking if match succeeded

**Code Example - WRONG:**
```python
import re

# Not checking if match found
text = "Hello World"
match = re.search(r'xyz', text)
print(match.group())  # ERROR! match is None

# Using wrong method result
matches = re.findall(r'world', text)
print(matches.group())  # ERROR! findall returns list, not match object

# Assuming match always succeeds
email = "not-an-email"
match = re.search(r'[\w.]+@[\w.]+', email)
domain = match.group()  # ERROR if no match
```

**Code Example - CORRECT:**
```python
import re

# Check if match exists
text = "Hello World"
match = re.search(r'xyz', text)
if match:
    print(match.group())
else:
    print("Not found")  # ✓

# Use findall correctly (returns list)
matches = re.findall(r'World', text)
if matches:
    print(matches[0])  # ✓ Access list element

# Safe pattern with default
email = "not-an-email"
match = re.search(r'[\w.]+@[\w.]+', email)
domain = match.group() if match else "invalid"  # ✓

# Use try/except
try:
    match = re.search(r'pattern', text)
    result = match.group()
except AttributeError:
    result = None  # ✓

# Helper function
def safe_search(pattern, text, default=""):
    """Safely search with default"""
    match = re.search(pattern, text)
    return match.group() if match else default

result = safe_search(r'xyz', text, default="not found")  # ✓
```

---

## 11.4 Groups and Capturing

### Extracting Parts

```python
import re

# Basic grouping
text = "John: 30"
match = re.search(r'(\w+): (\d+)', text)
if match:
    name = match.group(1)  # "John"
    age = match.group(2)   # "30"
    full = match.group(0)  # "John: 30" (entire match)

# Named groups
match = re.search(r'(?P<name>\w+): (?P<age>\d+)', text)
if match:
    name = match.group('name')  # "John"
    age = match.group('age')    # "30"

# Extract email parts
email = "user@example.com"
match = re.search(r'(?P<user>[\w.]+)@(?P<domain>[\w.]+)', email)
if match:
    user = match.group('user')      # "user"
    domain = match.group('domain')  # "example.com"

# Multiple matches with groups
text = "John:30, Jane:25, Bob:35"
matches = re.findall(r'(\w+):(\d+)', text)
for name, age in matches:
    print(f"{name} is {age}")
```

---

## 11.5 Greedy vs Non-Greedy

### Understanding Quantifiers

```python
import re

text = "<div>content</div>"

# Greedy (default) - matches as much as possible
match = re.search(r'<.*>', text)
print(match.group())  # "<div>content</div>" - entire string

# Non-greedy - matches as little as possible
match = re.search(r'<.*?>', text)
print(match.group())  # "<div>" - stops at first >

# Examples
text = "aaa"
re.findall(r'a+', text)   # ['aaa'] - greedy
re.findall(r'a+?', text)  # ['a', 'a', 'a'] - non-greedy

text = '123'
re.findall(r'\d{2,4}', text)   # ['123'] - greedy (max 4)
re.findall(r'\d{2,4}?', text)  # ['12'] - non-greedy (min 2)

# Practical example - extract HTML tags
html = "<p>First</p><p>Second</p>"
re.findall(r'<p>.*?</p>', html)  # ✓ ['<p>First</p>', '<p>Second</p>']
re.findall(r'<p>.*</p>', html)   # ['<p>First</p><p>Second</p>'] - greedy
```

---

## 11.6 Substitution

### Replacing Patterns

```python
import re

# Simple replacement
text = "Hello World"
result = re.sub(r'World', 'Python', text)
print(result)  # "Hello Python"

# Replace with function
def uppercase(match):
    return match.group().upper()

text = "hello world"
result = re.sub(r'\w+', uppercase, text)
print(result)  # "HELLO WORLD"

# Replace using groups
text = "John Doe"
result = re.sub(r'(\w+) (\w+)', r'\2, \1', text)
print(result)  # "Doe, John"

# Replace with named groups
result = re.sub(r'(?P<first>\w+) (?P<last>\w+)', 
                r'\g<last>, \g<first>', text)
print(result)  # "Doe, John"

# Limit replacements
text = "cat bat cat rat"
result = re.sub(r'cat', 'dog', text, count=1)
print(result)  # "dog bat cat rat"

# Case-insensitive replacement
result = re.sub(r'WORLD', 'Python', 'Hello WORLD', flags=re.IGNORECASE)
print(result)  # "Hello Python"
```

---

## 11.7 Flags

### Regex Modifiers

```python
import re

# Case-insensitive
text = "Hello WORLD"
match = re.search(r'world', text, re.IGNORECASE)
# Or: re.IGNORECASE, re.I

# Multiline - ^ and $ match line boundaries
text = "line1\nline2\nline3"
matches = re.findall(r'^line', text, re.MULTILINE)
# Matches: ['line', 'line', 'line']

# Dotall - . matches newlines
text = "first\nsecond"
match = re.search(r'first.second', text, re.DOTALL)
# Matches across newline

# Verbose - allows comments and whitespace
pattern = r'''
    \d{3}  # Area code
    -      # Separator
    \d{3}  # Prefix
    -      # Separator
    \d{4}  # Line number
'''
match = re.search(pattern, '555-123-4567', re.VERBOSE)

# Combine flags
match = re.search(r'pattern', text, re.IGNORECASE | re.MULTILINE)
```

---

## 11.8 Common Patterns and Use Cases

### Practical Examples

```python
import re

# Validate email
def is_valid_email(email):
    pattern = r'^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$'
    return re.match(pattern, email) is not None

# Validate phone number (US)
def is_valid_phone(phone):
    pattern = r'^\d{3}-\d{3}-\d{4}$'
    return re.match(pattern, phone) is not None

# Extract URLs from text
def extract_urls(text):
    pattern = r'https?://[^\s]+'
    return re.findall(pattern, text)

# Remove HTML tags
def remove_html_tags(html):
    pattern = r'<[^>]+>'
    return re.sub(pattern, '', html)

# Extract numbers from string
def extract_numbers(text):
    pattern = r'\d+'
    return [int(x) for x in re.findall(pattern, text)]

# Validate password (8+ chars, letter, number)
def is_strong_password(password):
    if len(password) < 8:
        return False
    if not re.search(r'[a-zA-Z]', password):
        return False
    if not re.search(r'\d', password):
        return False
    return True

# Parse log line
def parse_log_line(line):
    pattern = r'(?P<date>\S+) (?P<time>\S+) (?P<level>\w+) (?P<message>.*)'
    match = re.match(pattern, line)
    if match:
        return match.groupdict()
    return None
```

---

## 11.9 Practice Problems

### Problem 1: Invalid Pattern
```python
import re
pattern = '(hello'
re.search(pattern, 'hello world')
```

<details>
<summary>Click for Answer</summary>

**Error:** `re.error: missing ), unterminated subpattern`

**Fix:**
```python
import re
pattern = r'(hello)'  # ✓ Close parentheses
match = re.search(pattern, 'hello world')
```
</details>

---

### Problem 2: NoneType Error
```python
import re
text = "Hello World"
match = re.search(r'xyz', text)
print(match.group())
```

<details>
<summary>Click for Answer</summary>

**Error:** `AttributeError: 'NoneType' object has no attribute 'group'`

**Fix:**
```python
import re
text = "Hello World"
match = re.search(r'xyz', text)
if match:  # ✓ Check if found
    print(match.group())
else:
    print("Not found")
```
</details>

---

### Problem 3: Greedy Match
```python
import re
html = "<p>First</p><p>Second</p>"
matches = re.findall(r'<p>.*</p>', html)
print(matches)
```

<details>
<summary>Click for Answer</summary>

**Issue:** Returns entire string (greedy)

**Fix:**
```python
import re
html = "<p>First</p><p>Second</p>"
matches = re.findall(r'<p>.*?</p>', html)  # ✓ Non-greedy
print(matches)  # ['<p>First</p>', '<p>Second</p>']
```
</details>

---

## 11.10 Key Takeaways

### What You Learned

1. **Use raw strings** - r'' for regex patterns
2. **Check match results** - Before calling .group()
3. **Escape special characters** - \. \$ \( etc.
4. **Use non-greedy** - .*? for minimal matching
5. **Validate patterns** - Test with re.compile()
6. **Use named groups** - (?P<name>...) for clarity
7. **Choose right method** - match/search/findall/sub

### Common Patterns

```python
# Pattern 1: Safe search
match = re.search(pattern, text)
if match:
    result = match.group()

# Pattern 2: Extract all
matches = re.findall(pattern, text)

# Pattern 3: Replace
result = re.sub(pattern, replacement, text)

# Pattern 4: Validate
def is_valid(text):
    return re.match(pattern, text) is not None
```

### Error Summary

| Error | Cause | Prevention |
|-------|-------|------------|
| `re.error` | Invalid pattern syntax | Use raw strings, close brackets |
| `AttributeError` | match is None | Check if match before .group() |
| Greedy issues | Using .* | Use .*? for non-greedy |

---

## 11.11 Moving Forward

You now understand regular expressions! In **Chapter 12**, we'll explore **Pandas Basics** - data analysis with DataFrames!

