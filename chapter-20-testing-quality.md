# Chapter 20: Testing and Code Quality - Writing Better Code

## Introduction

**Testing and code quality** ensure your code works correctly and is maintainable. This final chapter covers testing frameworks, best practices, and tools for writing professional Python code.

Topics covered:
- Unit testing
- Test-driven development
- Code quality tools
- Best practices

Let's complete your journey!

---

## 20.1 Unit Testing Basics

### Using unittest

```python
import unittest

# Code to test
def add(a, b):
    return a + b

def divide(a, b):
    if b == 0:
        raise ValueError("Cannot divide by zero")
    return a / b

# Test class
class TestMath(unittest.TestCase):
    
    def test_add(self):
        self.assertEqual(add(2, 3), 5)
        self.assertEqual(add(-1, 1), 0)
        self.assertEqual(add(0, 0), 0)
    
    def test_divide(self):
        self.assertEqual(divide(10, 2), 5)
        self.assertEqual(divide(9, 3), 3)
    
    def test_divide_by_zero(self):
        with self.assertRaises(ValueError):
            divide(10, 0)
    
    def test_types(self):
        self.assertIsInstance(add(1, 2), int)
        self.assertTrue(add(1, 1) > 0)
        self.assertFalse(add(0, 0) > 0)

# Run tests
if __name__ == '__main__':
    unittest.main()
```

---

## 20.2 Common Test Assertions

### unittest Assertions

```python
import unittest

class TestAssertions(unittest.TestCase):
    
    def test_equality(self):
        self.assertEqual(1, 1)
        self.assertNotEqual(1, 2)
    
    def test_truth(self):
        self.assertTrue(True)
        self.assertFalse(False)
    
    def test_none(self):
        self.assertIsNone(None)
        self.assertIsNotNone("value")
    
    def test_membership(self):
        self.assertIn(1, [1, 2, 3])
        self.assertNotIn(4, [1, 2, 3])
    
    def test_types(self):
        self.assertIsInstance(1, int)
        self.assertIsInstance("text", str)
    
    def test_comparisons(self):
        self.assertGreater(2, 1)
        self.assertLess(1, 2)
        self.assertGreaterEqual(2, 2)
        self.assertLessEqual(1, 2)
    
    def test_sequences(self):
        self.assertListEqual([1, 2], [1, 2])
        self.assertDictEqual({'a': 1}, {'a': 1})
    
    def test_exceptions(self):
        with self.assertRaises(ValueError):
            int("abc")
        
        with self.assertRaises(ZeroDivisionError):
            1 / 0
    
    def test_almost_equal(self):
        self.assertAlmostEqual(0.1 + 0.2, 0.3)
```

---

## 20.3 pytest Framework

### Using pytest

```python
# test_math.py
import pytest

def add(a, b):
    return a + b

def divide(a, b):
    if b == 0:
        raise ValueError("Cannot divide by zero")
    return a / b

# Tests (no class needed)
def test_add():
    assert add(2, 3) == 5
    assert add(-1, 1) == 0

def test_divide():
    assert divide(10, 2) == 5
    assert divide(9, 3) == 3

def test_divide_by_zero():
    with pytest.raises(ValueError):
        divide(10, 0)

# Parametrized tests
@pytest.mark.parametrize("a,b,expected", [
    (2, 3, 5),
    (-1, 1, 0),
    (0, 0, 0),
    (100, 200, 300),
])
def test_add_parametrized(a, b, expected):
    assert add(a, b) == expected

# Fixtures
@pytest.fixture
def sample_data():
    return [1, 2, 3, 4, 5]

def test_with_fixture(sample_data):
    assert len(sample_data) == 5
    assert sum(sample_data) == 15

# Run: pytest test_math.py
```

---

## 20.4 Test Organization

### Structuring Tests

```python
# Project structure
"""
myproject/
    mypackage/
        __init__.py
        module1.py
        module2.py
    tests/
        __init__.py
        test_module1.py
        test_module2.py
    setup.py
    README.md
"""

# test_module1.py
import unittest
from mypackage import module1

class TestModule1(unittest.TestCase):
    
    def setUp(self):
        """Run before each test"""
        self.data = [1, 2, 3]
    
    def tearDown(self):
        """Run after each test"""
        self.data = None
    
    def test_function1(self):
        result = module1.function1(self.data)
        self.assertEqual(result, expected)
    
    def test_function2(self):
        result = module1.function2(self.data)
        self.assertTrue(result)

# Run all tests
# python -m unittest discover tests
# pytest tests/
```

---

## 20.5 Test-Driven Development (TDD)

### TDD Workflow

```python
"""
TDD Cycle:
1. Write test (it fails - Red)
2. Write code (make it pass - Green)
3. Refactor (improve code - Refactor)
4. Repeat
"""

# Example: TDD for a function

# Step 1: Write test first
def test_calculate_discount():
    assert calculate_discount(100, 10) == 90
    assert calculate_discount(50, 20) == 40

# Step 2: Run test (fails - function doesn't exist)

# Step 3: Write minimal code
def calculate_discount(price, discount_percent):
    return price - (price * discount_percent / 100)

# Step 4: Run test (passes)

# Step 5: Add more tests
def test_calculate_discount_edge_cases():
    assert calculate_discount(100, 0) == 100
    assert calculate_discount(100, 100) == 0
    with pytest.raises(ValueError):
        calculate_discount(100, -10)

# Step 6: Update code
def calculate_discount(price, discount_percent):
    if discount_percent < 0 or discount_percent > 100:
        raise ValueError("Discount must be 0-100")
    return price - (price * discount_percent / 100)

# Step 7: Refactor if needed
```

---

## 20.6 Code Coverage

### Measuring Test Coverage

```python
# Install coverage
# pip install coverage

# Run tests with coverage
# coverage run -m pytest

# View report
# coverage report

# Generate HTML report
# coverage html
# open htmlcov/index.html

# .coveragerc configuration
"""
[run]
source = mypackage
omit = 
    */tests/*
    */venv/*

[report]
exclude_lines =
    pragma: no cover
    def __repr__
    raise NotImplementedError
    if __name__ == .__main__.:
"""

# Aim for high coverage
# 80%+ is good
# 100% is ideal but not always necessary
```

---

## 20.7 Code Quality Tools

### Linting and Formatting

```python
# pylint - Code analysis
# pip install pylint
# pylint mymodule.py

# flake8 - Style guide enforcement
# pip install flake8
# flake8 mymodule.py

# black - Code formatter
# pip install black
# black mymodule.py

# mypy - Type checking
# pip install mypy
# mypy mymodule.py

# Example with type hints
def add(a: int, b: int) -> int:
    """Add two integers"""
    return a + b

# isort - Import sorting
# pip install isort
# isort mymodule.py

# Example .flake8 config
"""
[flake8]
max-line-length = 88
extend-ignore = E203, W503
exclude = 
    .git,
    __pycache__,
    venv
"""

# Example pyproject.toml for black
"""
[tool.black]
line-length = 88
target-version = ['py38']
include = '\.pyi?$'
"""
```

---

## 20.8 Best Practices

### Writing Quality Code

```python
# 1. Write docstrings
def calculate_total(items: list, tax_rate: float) -> float:
    """
    Calculate total price including tax.
    
    Args:
        items: List of items with 'price' key
        tax_rate: Tax rate as decimal (0.08 = 8%)
    
    Returns:
        Total price including tax
    
    Raises:
        ValueError: If tax_rate is negative
    
    Example:
        >>> items = [{'price': 10}, {'price': 20}]
        >>> calculate_total(items, 0.08)
        32.4
    """
    if tax_rate < 0:
        raise ValueError("Tax rate cannot be negative")
    
    subtotal = sum(item['price'] for item in items)
    return subtotal * (1 + tax_rate)

# 2. Use type hints
from typing import List, Dict, Optional

def process_data(
    data: List[Dict[str, any]],
    filter_key: Optional[str] = None
) -> List[Dict[str, any]]:
    """Process data with optional filtering"""
    if filter_key:
        return [d for d in data if filter_key in d]
    return data

# 3. Follow PEP 8
# - 4 spaces for indentation
# - 2 blank lines between functions
# - Lowercase with underscores for functions
# - CamelCase for classes

# ✓ Good
def calculate_average(numbers):
    return sum(numbers) / len(numbers)

class DataProcessor:
    pass

# ✗ Bad
def calculateAverage(numbers):  # camelCase
    return sum(numbers)/len(numbers)  # no spaces

# 4. Keep functions small
# ✓ Good - One responsibility
def validate_email(email):
    return '@' in email and '.' in email

def send_email(email, message):
    if not validate_email(email):
        raise ValueError("Invalid email")
    # Send email

# ✗ Bad - Too many responsibilities
def process_user_registration(email, password, name):
    # Validate email
    # Validate password
    # Hash password
    # Save to database
    # Send confirmation email
    # Log activity
    pass  # Too much!

# 5. Use meaningful names
# ✓ Good
user_age = 25
total_price = calculate_total(items)
is_valid = validate_input(data)

# ✗ Bad
x = 25
tmp = calc(items)
flag = check(data)

# 6. Don't repeat yourself (DRY)
# ✗ Bad
def calculate_area_rectangle(width, height):
    return width * height

def calculate_area_square(side):
    return side * side

# ✓ Good
def calculate_area(width, height=None):
    if height is None:
        height = width
    return width * height

# 7. Handle errors properly
# ✓ Good
def read_file(filename):
    try:
        with open(filename, 'r') as f:
            return f.read()
    except FileNotFoundError:
        logger.error(f"File not found: {filename}")
        raise
    except Exception as e:
        logger.error(f"Error reading {filename}: {e}")
        raise

# ✗ Bad
def read_file(filename):
    try:
        with open(filename, 'r') as f:
            return f.read()
    except:
        pass  # Silent failure!
```

---

## 20.9 Continuous Integration

### CI/CD Setup

```python
# GitHub Actions example
# .github/workflows/tests.yml
"""
name: Tests

on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest
    
    steps:
    - uses: actions/checkout@v2
    
    - name: Set up Python
      uses: actions/setup-python@v2
      with:
        python-version: 3.9
    
    - name: Install dependencies
      run: |
        pip install -r requirements.txt
        pip install pytest coverage
    
    - name: Run tests
      run: |
        pytest
    
    - name: Check coverage
      run: |
        coverage run -m pytest
        coverage report --fail-under=80
    
    - name: Lint
      run: |
        pip install flake8
        flake8 .
"""

# pre-commit hooks
# .pre-commit-config.yaml
"""
repos:
  - repo: https://github.com/psf/black
    rev: 23.1.0
    hooks:
      - id: black
  
  - repo: https://github.com/pycqa/flake8
    rev: 6.0.0
    hooks:
      - id: flake8
  
  - repo: https://github.com/pycqa/isort
    rev: 5.12.0
    hooks:
      - id: isort
"""
```

---

## 20.10 Final Checklist

### Code Quality Checklist

```python
"""
Before committing code:

✓ Tests pass
  - All unit tests pass
  - Coverage > 80%
  - Edge cases covered

✓ Code quality
  - No linter warnings
  - Code formatted (black)
  - Imports sorted (isort)
  - Type hints added

✓ Documentation
  - Docstrings added
  - README updated
  - CHANGELOG updated

✓ Best practices
  - No duplicate code
  - Functions < 20 lines
  - No magic numbers
  - Error handling

✓ Review
  - Self-review changes
  - Test locally
  - Check CI passes
"""
```

---

## 20.11 Key Takeaways

### What You Learned

1. **Write tests first** - TDD approach
2. **Test edge cases** - Not just happy path
3. **Aim for high coverage** - 80%+ is good
4. **Use quality tools** - Linters, formatters
5. **Follow PEP 8** - Consistent style
6. **Write docstrings** - Document your code
7. **Keep it simple** - KISS principle

### Quality Pyramid

```
         /\
        /CI\
       /----\
      /Tests \
     /--------\
    /  Linting \
   /------------\
  / Code Reviews \
 /----------------\
```

---

## 20.12 Congratulations! 🎉

### You've Completed the Python Error Guide!

You've mastered all 20 chapters:

**Part I: Fundamentals**
- Variables, Operators, Strings
- Lists, Dictionaries, Sets, Tuples
- Conditionals, Loops, Functions, Files

**Part II: Libraries**
- Regular Expressions
- Pandas, NumPy, Matplotlib

**Part III: Advanced**
- OOP, Modules, Exceptions
- Debugging, Testing, Quality

---

## 20.13 Next Steps

### Continue Your Python Journey

1. **Practice regularly**
   - Code every day
   - Build projects
   - Contribute to open source

2. **Read quality code**
   - Study Python standard library
   - Read popular projects
   - Learn from experts

3. **Stay updated**
   - Follow Python PEPs
   - Read blogs and articles
   - Join Python communities

4. **Specialize**
   - Web (Django, Flask)
   - Data Science (Pandas, scikit-learn)
   - DevOps (automation)
   - ML/AI (TensorFlow, PyTorch)

5. **Teach others**
   - Write blog posts
   - Answer questions
   - Mentor beginners

---

## 20.14 Resources

### Recommended Learning

**Books:**
- "Fluent Python" by Luciano Ramalho
- "Effective Python" by Brett Slatkin
- "Python Tricks" by Dan Bader

**Websites:**
- Real Python (realpython.com)
- Python Docs (docs.python.org)
- PEP 8 Style Guide

**Practice:**
- LeetCode
- HackerRank
- Project Euler

**Communities:**
- r/Python
- Python Discord
- Stack Overflow

---

## 🎊 Thank You!

You've completed this comprehensive guide to Python errors and best practices. You now have the knowledge to:

- ✅ Understand and fix Python errors
- ✅ Write clean, maintainable code
- ✅ Test your code properly
- ✅ Debug effectively
- ✅ Follow best practices

**Keep coding, keep learning, and keep growing!** 🐍💙

---

*End of Python Error Guide*
*Thank you for learning with us!*
