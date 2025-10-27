# Chapter 10: File I/O - Reading and Writing Files

## Introduction

You've mastered functions. Now let's explore **File I/O** (Input/Output) - reading from and writing to files. Working with files is essential for data persistence, configuration, logging, and data processing.

Common errors:
- **FileNotFoundError**: File doesn't exist
- **PermissionError**: No access to file
- **IsADirectoryError**: Path is directory, not file
- **IOError**: Input/output errors
- Encoding issues

Let's master file operations!

---

## 10.1 Reading Files

### Basic File Reading

```python
# Read entire file
with open('file.txt', 'r') as file:
    content = file.read()
    print(content)

# Read line by line
with open('file.txt', 'r') as file:
    for line in file:
        print(line.strip())  # strip() removes \n

# Read all lines into list
with open('file.txt', 'r') as file:
    lines = file.readlines()  # List of lines

# Read specific number of characters
with open('file.txt', 'r') as file:
    chunk = file.read(100)  # First 100 characters
```

---

### Error Type 1: `FileNotFoundError: No such file or directory`

**Error Message:**
```python
>>> with open('nonexistent.txt', 'r') as file:
...     content = file.read()
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
FileNotFoundError: [Errno 2] No such file or directory: 'nonexistent.txt'
```

**What Happened:**
Trying to open a file that doesn't exist.

**Why It Happens:**
- File doesn't exist at path
- Wrong file name or path
- Working directory confusion
- Typo in filename

**Code Example - WRONG:**
```python
# File doesn't exist
with open('nonexistent.txt', 'r') as file:
    content = file.read()  # ERROR!

# Wrong path
with open('/wrong/path/file.txt', 'r') as file:
    content = file.read()  # ERROR!

# Typo in filename
with open('flie.txt', 'r') as file:  # Typo: flie
    content = file.read()  # ERROR!

# Case sensitivity on Linux/Mac
with open('File.txt', 'r') as file:  # file.txt exists
    content = file.read()  # ERROR on case-sensitive systems
```

**Code Example - CORRECT:**
```python
import os

# Check if file exists first
if os.path.exists('file.txt'):
    with open('file.txt', 'r') as file:
        content = file.read()  # ✓
else:
    print("File not found")

# Use try/except
try:
    with open('file.txt', 'r') as file:
        content = file.read()
except FileNotFoundError:
    print("File not found")  # ✓
    content = ""  # Default value

# Use absolute path
file_path = '/home/user/documents/file.txt'
if os.path.exists(file_path):
    with open(file_path, 'r') as file:
        content = file.read()

# Check current directory
print("Current directory:", os.getcwd())
print("Files:", os.listdir())  # List files

# Use os.path.join for cross-platform paths
file_path = os.path.join('data', 'file.txt')
if os.path.exists(file_path):
    with open(file_path, 'r') as file:
        content = file.read()

# Create file if doesn't exist (for writing)
with open('file.txt', 'a') as file:  # 'a' creates if needed
    file.write("Content")
```

---

## 10.2 Writing Files

### Basic File Writing

```python
# Write to file (overwrites)
with open('output.txt', 'w') as file:
    file.write("Hello, World!")

# Append to file
with open('output.txt', 'a') as file:
    file.write("\nNew line")

# Write multiple lines
lines = ["Line 1\n", "Line 2\n", "Line 3\n"]
with open('output.txt', 'w') as file:
    file.writelines(lines)

# Write with print (adds newline)
with open('output.txt', 'w') as file:
    print("Hello", file=file)
    print("World", file=file)
```

---

### Error Type 2: `PermissionError: Permission denied`

**Error Message:**
```python
>>> with open('/root/file.txt', 'w') as file:
...     file.write("Content")
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
PermissionError: [Errno 13] Permission denied: '/root/file.txt'
```

**What Happened:**
No permission to read/write file.

**Why It Happens:**
- Insufficient permissions
- File is read-only
- Directory doesn't allow writes
- File is locked by another program

**Code Example - WRONG:**
```python
# No permission to write
with open('/root/file.txt', 'w') as file:
    file.write("Content")  # ERROR!

# File is read-only
with open('readonly.txt', 'w') as file:
    file.write("Content")  # ERROR!

# Directory doesn't exist
with open('/nonexistent/dir/file.txt', 'w') as file:
    file.write("Content")  # ERROR!
```

**Code Example - CORRECT:**
```python
import os

# Check write permission
file_path = 'file.txt'
try:
    with open(file_path, 'w') as file:
        file.write("Content")
except PermissionError:
    print("No permission to write")  # ✓

# Check if directory is writable
dir_path = '/some/directory'
if os.access(dir_path, os.W_OK):
    file_path = os.path.join(dir_path, 'file.txt')
    with open(file_path, 'w') as file:
        file.write("Content")
else:
    print("Directory not writable")

# Create directory if needed
dir_path = 'data'
os.makedirs(dir_path, exist_ok=True)  # ✓ Creates if needed
file_path = os.path.join(dir_path, 'file.txt')
with open(file_path, 'w') as file:
    file.write("Content")

# Write to user's home directory (usually writable)
import os.path
home = os.path.expanduser('~')
file_path = os.path.join(home, 'file.txt')
with open(file_path, 'w') as file:
    file.write("Content")  # ✓

# Write to temp directory
import tempfile
with tempfile.NamedTemporaryFile(mode='w', delete=False) as file:
    file.write("Content")
    temp_path = file.name  # ✓ Always writable
```

---

## 10.3 Context Managers (with statement)

### Understanding 'with'

```python
# WITHOUT 'with' (manual close)
file = open('file.txt', 'r')
try:
    content = file.read()
finally:
    file.close()  # Must close manually

# WITH 'with' (automatic close)
with open('file.txt', 'r') as file:
    content = file.read()
# File automatically closed ✓

# Multiple files
with open('input.txt', 'r') as infile, \
     open('output.txt', 'w') as outfile:
    content = infile.read()
    outfile.write(content)
# Both files closed automatically ✓
```

---

### Error Type 3: Forgetting to Close Files

**What Happened:**
Not closing files can lead to data loss and resource leaks.

**Code Example - WRONG:**
```python
# Not closing file
file = open('file.txt', 'w')
file.write("Content")
# ERROR! File not closed, data might not be saved

# Exception prevents close
file = open('file.txt', 'r')
content = file.read()
process(content)  # If this raises exception...
file.close()  # ...this never runs

# Closing in wrong place
file = open('file.txt', 'r')
for line in file:
    print(line)
    file.close()  # ERROR! Closes after first line
```

**Code Example - CORRECT:**
```python
# Use 'with' statement (BEST)
with open('file.txt', 'w') as file:
    file.write("Content")
# File automatically closed ✓

# Use try/finally if not using 'with'
file = open('file.txt', 'r')
try:
    content = file.read()
    process(content)
finally:
    file.close()  # ✓ Always closes

# Correct loop
with open('file.txt', 'r') as file:
    for line in file:
        print(line)
# File closed after loop ✓

# Flush to ensure write
with open('file.txt', 'w') as file:
    file.write("Important data")
    file.flush()  # ✓ Forces write to disk
```

---

## 10.4 File Modes

### Understanding File Modes

```python
# Read modes
'r'   # Read (default) - error if doesn't exist
'rb'  # Read binary
'r+'  # Read and write

# Write modes
'w'   # Write - creates new or overwrites
'wb'  # Write binary
'w+'  # Write and read

# Append modes
'a'   # Append - creates if doesn't exist
'ab'  # Append binary
'a+'  # Append and read

# Exclusive creation
'x'   # Create new - error if exists
```

---

### Error Type 4: `FileExistsError: File exists`

**Error Message:**
```python
>>> with open('existing.txt', 'x') as file:
...     file.write("Content")
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
FileExistsError: [Errno 17] File exists: 'existing.txt'
```

**What Happened:**
Using 'x' mode on existing file.

**Code Example - WRONG:**
```python
# 'x' mode with existing file
with open('existing.txt', 'x') as file:
    file.write("Content")  # ERROR if file exists

# Wrong mode for operation
with open('file.txt', 'r') as file:
    file.write("Content")  # ERROR! Can't write in 'r' mode

# Binary mode with string
with open('file.txt', 'wb') as file:
    file.write("Hello")  # ERROR! Need bytes, not string
```

**Code Example - CORRECT:**
```python
# Check before using 'x'
if not os.path.exists('file.txt'):
    with open('file.txt', 'x') as file:
        file.write("Content")  # ✓
else:
    print("File already exists")

# Use appropriate mode
with open('file.txt', 'w') as file:  # ✓ For writing
    file.write("Content")

with open('file.txt', 'r') as file:  # ✓ For reading
    content = file.read()

# Binary mode with bytes
with open('file.txt', 'wb') as file:
    file.write(b"Hello")  # ✓ Bytes object

# Or encode string
with open('file.txt', 'wb') as file:
    file.write("Hello".encode('utf-8'))  # ✓

# Text mode (default)
with open('file.txt', 'w') as file:
    file.write("Hello")  # ✓ String

# Safe overwrite pattern
import shutil
if os.path.exists('file.txt'):
    shutil.copy('file.txt', 'file.txt.bak')  # Backup
with open('file.txt', 'w') as file:
    file.write("New content")  # ✓
```

---

## 10.5 Working with Paths

### Path Operations

```python
import os

# Current directory
current = os.getcwd()

# Change directory
os.chdir('/path/to/directory')

# Join paths (cross-platform)
path = os.path.join('data', 'files', 'document.txt')
# Windows: data\files\document.txt
# Unix: data/files/document.txt

# Split path
directory, filename = os.path.split('/path/to/file.txt')
# directory: '/path/to'
# filename: 'file.txt'

# Get filename and extension
filename, ext = os.path.splitext('document.txt')
# filename: 'document'
# ext: '.txt'

# Check existence
os.path.exists('file.txt')      # File or directory
os.path.isfile('file.txt')      # File only
os.path.isdir('directory')      # Directory only

# Get absolute path
abs_path = os.path.abspath('file.txt')

# List directory contents
files = os.listdir('directory')

# Create directory
os.makedirs('path/to/directory', exist_ok=True)
```

---

## 10.6 Common File Patterns

### Useful Patterns

```python
# Read file safely
def read_file(filename):
    try:
        with open(filename, 'r') as file:
            return file.read()
    except FileNotFoundError:
        return None

# Write file safely
def write_file(filename, content):
    try:
        with open(filename, 'w') as file:
            file.write(content)
        return True
    except (PermissionError, IOError):
        return False

# Copy file
def copy_file(source, destination):
    with open(source, 'r') as infile:
        with open(destination, 'w') as outfile:
            outfile.write(infile.read())

# Process file line by line
def process_large_file(filename):
    with open(filename, 'r') as file:
        for line in file:  # Memory efficient
            process_line(line.strip())

# Read CSV
def read_csv(filename):
    data = []
    with open(filename, 'r') as file:
        for line in file:
            data.append(line.strip().split(','))
    return data

# Write CSV
def write_csv(filename, data):
    with open(filename, 'w') as file:
        for row in data:
            file.write(','.join(str(x) for x in row) + '\n')
```

---

## 10.7 Practice Problems

### Problem 1: File Not Found
```python
with open('data.txt', 'r') as file:
    content = file.read()
```

<details>
<summary>Click for Answer</summary>

**Error:** `FileNotFoundError: No such file or directory: 'data.txt'`

**Fix:**
```python
import os

# Check first
if os.path.exists('data.txt'):
    with open('data.txt', 'r') as file:
        content = file.read()
else:
    print("File not found")  # ✓

# Or use try/except
try:
    with open('data.txt', 'r') as file:
        content = file.read()
except FileNotFoundError:
    content = ""  # ✓ Default
```
</details>

---

### Problem 2: Not Closing File
```python
file = open('output.txt', 'w')
file.write("Content")
```

<details>
<summary>Click for Answer</summary>

**Issue:** File not closed, data might not be saved

**Fix:**
```python
# Use 'with' statement
with open('output.txt', 'w') as file:
    file.write("Content")
# File automatically closed ✓
```
</details>

---

### Problem 3: Wrong Mode
```python
with open('file.txt', 'r') as file:
    file.write("New content")
```

<details>
<summary>Click for Answer</summary>

**Error:** `io.UnsupportedOperation: not writable`

**Fix:**
```python
# Use write mode
with open('file.txt', 'w') as file:  # ✓ Use 'w'
    file.write("New content")

# Or append mode
with open('file.txt', 'a') as file:  # ✓ Use 'a'
    file.write("New content")
```
</details>

---

## 10.8 Key Takeaways

### What You Learned

1. **Check file exists** - Before opening for reading
2. **Use 'with' statement** - Automatically closes files
3. **Handle exceptions** - FileNotFoundError, PermissionError
4. **Use correct mode** - 'r' for read, 'w' for write, 'a' for append
5. **Use os.path.join** - Cross-platform paths
6. **Close files** - Or use 'with' to auto-close
7. **Check permissions** - Before writing

### Common Patterns

```python
# Pattern 1: Safe read
try:
    with open('file.txt', 'r') as file:
        content = file.read()
except FileNotFoundError:
    content = ""

# Pattern 2: Safe write
with open('file.txt', 'w') as file:
    file.write(content)

# Pattern 3: Check exists
if os.path.exists('file.txt'):
    # Process file

# Pattern 4: Create path
os.makedirs('path/to/dir', exist_ok=True)
```

### Error Summary

| Error | Cause | Prevention |
|-------|-------|------------|
| `FileNotFoundError` | File doesn't exist | Check with os.path.exists() |
| `PermissionError` | No access rights | Check permissions or use try/except |
| Not closing | Forgot to close | Use 'with' statement |
| Wrong mode | Incorrect r/w/a | Choose correct mode |

---

## 10.9 Congratulations!

### 🎉 You Completed Part I: Python Fundamentals!

You've mastered:
- ✅ Variables and Data Types (Chapter 1)
- ✅ Operators and Expressions (Chapter 2)
- ✅ Strings and String Methods (Chapter 3)
- ✅ Lists and List Methods (Chapter 4)
- ✅ Dictionaries and Sets (Chapter 5)
- ✅ Tuples and Immutability (Chapter 6)
- ✅ Conditional Statements (Chapter 7)
- ✅ Loops (Chapter 8)
- ✅ Functions (Chapter 9)
- ✅ File I/O (Chapter 10)

---

## 10.10 Moving Forward

**Part I Complete!** You now have a solid foundation in Python fundamentals.

**What's Next?**

### Part II: Libraries and Data (Chapters 11-15)
- Chapter 11: Regular Expressions
- Chapter 12: Pandas Basics
- Chapter 13: Pandas Advanced
- Chapter 14: NumPy
- Chapter 15: Matplotlib

### Part III: Advanced Topics (Chapters 16-20)
- Chapter 16: Object-Oriented Programming
- Chapter 17: Modules and Imports
- Chapter 18: Exception Handling
- Chapter 19: Debugging Techniques
- Chapter 20: Testing and Code Quality
