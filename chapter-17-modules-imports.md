# Chapter 17: Modules and Imports - Import Errors

## Introduction

**Modules** organize code into reusable files. Understanding imports is essential for using Python libraries and organizing your own code.

Common errors:
- **ModuleNotFoundError**: Module not installed or found
- **ImportError**: Can't import specific name
- **AttributeError**: Module attribute doesn't exist
- Circular imports

Let's master imports!

---

## 17.1 Basic Imports

### Import Syntax

```python
# Import entire module
import math
print(math.pi)  # 3.14159...

# Import specific function
from math import sqrt
print(sqrt(16))  # 4.0

# Import multiple
from math import pi, sqrt, ceil

# Import with alias
import numpy as np
arr = np.array([1, 2, 3])

# Import all (not recommended)
from math import *

# Import submodule
from os.path import join
path = join('folder', 'file.txt')
```

---

### Error Type 1: `ModuleNotFoundError: No module named 'module_name'`

**Error Message:**
```python
>>> import pandas
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ModuleNotFoundError: No module named 'pandas'
```

**What Happened:**
Module not installed or not in Python path.

**Why It Happens:**
- Module not installed
- Wrong module name
- Wrong Python environment
- Module in wrong location

**Code Example - WRONG:**
```python
# Module not installed
import pandas  # ERROR if not installed

# Typo in name
import nump  # ERROR! Should be numpy

# Wrong capitalization
import Pandas  # ERROR! Should be pandas

# Module doesn't exist
import my_nonexistent_module  # ERROR!
```

**Code Example - CORRECT:**
```python
# Install module first
# pip install pandas

import pandas  # ✓ After installation

# Check if module exists before importing
try:
    import pandas as pd
except ModuleNotFoundError:
    print("pandas not installed")  # ✓
    pd = None

# Use importlib to check
import importlib.util
spec = importlib.util.find_spec("pandas")
if spec is not None:
    import pandas  # ✓ Module exists
else:
    print("pandas not found")

# Correct module name
import numpy  # ✓ Correct spelling

# Check installed packages
# pip list
# pip show pandas

# Use correct Python environment
# python -m pip install pandas
# python3 -m pip install pandas

# Add to path if needed
import sys
sys.path.append('/path/to/module')  # ✓
import my_module
```

---

## 17.2 From Imports

### Importing Specific Names

```python
# Import specific function
from math import sqrt, pi

# Import class
from datetime import datetime

# Import with alias
from collections import defaultdict as dd

# Import from subpackage
from os.path import join, exists

# Multiple lines
from mymodule import (
    function1,
    function2,
    MyClass
)
```

---

### Error Type 2: `ImportError: cannot import name 'name' from 'module'`

**Error Message:**
```python
>>> from math import square_root
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ImportError: cannot import name 'square_root' from 'math'
```

**What Happened:**
Trying to import name that doesn't exist in module.

**Why It Happens:**
- Function/class doesn't exist
- Typo in name
- Wrong module
- Circular import

**Code Example - WRONG:**
```python
# Function doesn't exist
from math import square_root  # ERROR! It's sqrt

# Class doesn't exist
from datetime import Date  # ERROR! It's date

# Wrong module
from os import listdir  # Actually in os module, this works
from sys import listdir  # ERROR! Not in sys

# Typo
from math import squrt  # ERROR! Typo
```

**Code Example - CORRECT:**
```python
# Correct function name
from math import sqrt  # ✓

# Correct class name
from datetime import date  # ✓

# Check what's in module
import math
print(dir(math))  # ✓ List all names

# Check if name exists
if hasattr(math, 'sqrt'):
    from math import sqrt  # ✓
else:
    print("sqrt not in math")

# Try/except for import
try:
    from math import sqrt
except ImportError:
    print("Cannot import sqrt")  # ✓
    sqrt = None

# Import module, then access
import math
result = math.sqrt(16)  # ✓ Always works

# Check module documentation
help(math)  # ✓ See available names
```

---

## 17.3 Creating Your Own Modules

### Module Structure

```python
# mymodule.py
"""
My custom module
"""

# Module-level constant
PI = 3.14159

# Function
def greet(name):
    """Greet someone"""
    return f"Hello, {name}!"

# Class
class Calculator:
    """Simple calculator"""
    @staticmethod
    def add(x, y):
        return x + y

# Main execution guard
if __name__ == "__main__":
    print("Running as script")
    print(greet("World"))
```

```python
# Use the module (in another file)
import mymodule

print(mymodule.PI)
print(mymodule.greet("Alice"))
calc = mymodule.Calculator()
print(calc.add(5, 3))
```

---

### Error Type 3: Circular Import Error

**What Happened:**
Two modules import each other.

**Code Example - WRONG:**
```python
# module_a.py
from module_b import func_b

def func_a():
    return func_b()

# module_b.py
from module_a import func_a  # ERROR! Circular

def func_b():
    return func_a()

# main.py
import module_a  # ERROR! Circular import
```

**Code Example - CORRECT:**
```python
# Solution 1: Restructure to remove circular dependency
# module_a.py
def func_a():
    from module_b import func_b  # ✓ Import inside function
    return func_b()

# module_b.py
def func_b():
    return "result"

# Solution 2: Create third module
# common.py
def shared_function():
    return "shared"

# module_a.py
from common import shared_function

def func_a():
    return shared_function()

# module_b.py
from common import shared_function

def func_b():
    return shared_function()

# Solution 3: Use lazy import
# module_a.py
def func_a():
    import module_b  # ✓ Import when called
    return module_b.func_b()

# Solution 4: Import at bottom
# module_a.py
def func_a():
    return func_b()

from module_b import func_b  # ✓ After definition
```

---

## 17.4 Package Structure

### Creating Packages

```
mypackage/
    __init__.py
    module1.py
    module2.py
    subpackage/
        __init__.py
        module3.py
```

```python
# mypackage/__init__.py
"""Package initialization"""
from .module1 import function1
from .module2 import Class2

__all__ = ['function1', 'Class2']

# mypackage/module1.py
def function1():
    return "Function 1"

# mypackage/module2.py
class Class2:
    pass

# Usage
from mypackage import function1, Class2
from mypackage.subpackage import module3
```

---

## 17.5 Import Best Practices

### Guidelines

```python
# ✓ GOOD: Import order (PEP 8)
# 1. Standard library
import os
import sys
from datetime import datetime

# 2. Third-party
import numpy as np
import pandas as pd

# 3. Local/custom
from mymodule import myfunction

# ✓ GOOD: Specific imports
from math import sqrt, pi

# ✗ AVOID: Import *
from math import *  # Pollutes namespace

# ✓ GOOD: Clear aliases
import numpy as np
import pandas as pd

# ✗ AVOID: Unclear aliases
import numpy as n
import pandas as p

# ✓ GOOD: Group related imports
from os import (
    path,
    listdir,
    makedirs
)

# ✗ AVOID: Multiple statements per line
import sys, os  # Use separate lines

# ✓ GOOD: Absolute imports
from mypackage.subpackage import module

# ✓ GOOD: Relative imports (within package)
from . import sibling_module
from .. import parent_module
from ..sibling import cousin_module
```

---

## 17.6 Common Patterns

### Import Patterns

```python
# Conditional imports
try:
    import pandas as pd
    HAS_PANDAS = True
except ImportError:
    HAS_PANDAS = False

if HAS_PANDAS:
    # Use pandas
    df = pd.DataFrame()

# Version checking
import sys
if sys.version_info < (3, 6):
    raise RuntimeError("Python 3.6+ required")

# Dynamic imports
module_name = "math"
module = __import__(module_name)
result = module.sqrt(16)

# Or use importlib
import importlib
module = importlib.import_module("math")
result = module.sqrt(16)

# Lazy imports
class MyClass:
    def method(self):
        import expensive_module  # ✓ Only import when needed
        return expensive_module.function()

# Optional dependencies
try:
    import matplotlib.pyplot as plt
    CAN_PLOT = True
except ImportError:
    CAN_PLOT = False

def plot_data(data):
    if not CAN_PLOT:
        print("matplotlib not available")
        return
    plt.plot(data)
    plt.show()
```

---

## 17.7 Practice Problems

### Problem 1: Module Not Found
```python
import numpyy
```

<details>
<summary>Click for Answer</summary>

**Error:** `ModuleNotFoundError: No module named 'numpyy'`

**Fix:**
```python
import numpy  # ✓ Correct spelling

# Or install if needed
# pip install numpy
```
</details>

---

### Problem 2: Import Name Error
```python
from math import square_root
```

<details>
<summary>Click for Answer</summary>

**Error:** `ImportError: cannot import name 'square_root'`

**Fix:**
```python
from math import sqrt  # ✓ Correct name

# Check available names
import math
print(dir(math))  # ✓
```
</details>

---

## 17.8 Key Takeaways

### What You Learned

1. **Install before importing** - pip install package
2. **Check spelling** - Module names are case-sensitive
3. **Avoid circular imports** - Restructure code
4. **Use try/except** - For optional imports
5. **Import at top** - Unless lazy loading
6. **Follow PEP 8** - Import order and style
7. **Check with dir()** - See module contents

---

## 17.9 Moving Forward

You now understand imports! In **Chapter 18**, we'll explore **Exception Handling**!

