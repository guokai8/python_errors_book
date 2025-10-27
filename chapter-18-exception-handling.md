# Chapter 18: Exception Handling - try/except Patterns

## Introduction

**Exception handling** lets you gracefully manage errors instead of crashing. Proper exception handling is crucial for robust applications.

Common topics:
- try/except/finally blocks
- Multiple exception types
- Raising exceptions
- Custom exceptions
- Best practices

Let's master exception handling!

---

## 18.1 Basic Exception Handling

### try/except Blocks

```python
# Basic try/except
try:
    result = 10 / 0
except ZeroDivisionError:
    print("Cannot divide by zero")
    result = None

# Multiple exceptions
try:
    value = int("abc")
except ValueError:
    print("Invalid number")
except TypeError:
    print("Wrong type")

# Catch any exception
try:
    risky_operation()
except Exception as e:
    print(f"Error: {e}")

# Multiple exceptions in one block
try:
    operation()
except (ValueError, TypeError) as e:
    print(f"Error: {e}")

# Get exception details
try:
    result = 10 / 0
except ZeroDivisionError as e:
    print(f"Error type: {type(e)}")
    print(f"Error message: {e}")
```

---

## 18.2 else and finally

### Complete try Block

```python
# try/except/else/finally
try:
    file = open('data.txt', 'r')
    data = file.read()
except FileNotFoundError:
    print("File not found")
    data = None
else:
    print("File read successfully")  # Only if no exception
finally:
    print("Cleanup")  # Always executes
    if 'file' in locals():
        file.close()

# Common pattern
try:
    result = risky_operation()
except Exception as e:
    print(f"Error: {e}")
    result = default_value
else:
    print("Success")
finally:
    cleanup()

# finally for cleanup
file = None
try:
    file = open('data.txt', 'r')
    data = file.read()
except Exception as e:
    print(f"Error: {e}")
finally:
    if file:
        file.close()  # Always closes
```

---

## 18.3 Raising Exceptions

### Creating Exceptions

```python
# Raise exception
def divide(a, b):
    if b == 0:
        raise ValueError("Divisor cannot be zero")
    return a / b

# Re-raise exception
try:
    risky_operation()
except Exception as e:
    print(f"Logging error: {e}")
    raise  # Re-raise same exception

# Raise different exception
try:
    external_api_call()
except ExternalAPIError as e:
    raise RuntimeError("API failed") from e

# Raise with context
def process_data(data):
    if not data:
        raise ValueError("Data cannot be empty")
    if not isinstance(data, list):
        raise TypeError(f"Expected list, got {type(data)}")
    return process(data)
```

---

## 18.4 Custom Exceptions

### Creating Custom Exceptions

```python
# Basic custom exception
class MyCustomError(Exception):
    pass

raise MyCustomError("Something went wrong")

# With additional data
class ValidationError(Exception):
    def __init__(self, message, field=None):
        super().__init__(message)
        self.field = field

try:
    raise ValidationError("Invalid email", field="email")
except ValidationError as e:
    print(f"Error in {e.field}: {e}")

# Exception hierarchy
class AppError(Exception):
    """Base exception for app"""
    pass

class DatabaseError(AppError):
    """Database related errors"""
    pass

class NetworkError(AppError):
    """Network related errors"""
    pass

try:
    raise DatabaseError("Connection failed")
except DatabaseError as e:
    print("Database error")
except AppError as e:
    print("App error")

# With additional methods
class HTTPError(Exception):
    def __init__(self, status_code, message):
        self.status_code = status_code
        self.message = message
        super().__init__(self.message)
    
    def is_client_error(self):
        return 400 <= self.status_code < 500
    
    def is_server_error(self):
        return 500 <= self.status_code < 600

try:
    raise HTTPError(404, "Not Found")
except HTTPError as e:
    if e.is_client_error():
        print("Client error")
```

---

## 18.5 Exception Best Practices

### Good Patterns

```python
# ✓ GOOD: Specific exceptions
try:
    value = int(user_input)
except ValueError:  # Specific
    print("Invalid number")

# ✗ AVOID: Bare except
try:
    value = int(user_input)
except:  # Catches everything, even KeyboardInterrupt!
    print("Error")

# ✓ GOOD: Catch specific, then general
try:
    operation()
except ValueError:
    handle_value_error()
except TypeError:
    handle_type_error()
except Exception as e:
    handle_general_error(e)

# ✗ AVOID: Catch Exception first
try:
    operation()
except Exception:  # Too broad, catches everything
    pass
except ValueError:  # Never reached!
    pass

# ✓ GOOD: Don't suppress errors
try:
    important_operation()
except Exception as e:
    logger.error(f"Error: {e}")
    raise  # Re-raise

# ✗ AVOID: Silent failures
try:
    important_operation()
except:
    pass  # Error lost!

# ✓ GOOD: Use finally for cleanup
resource = None
try:
    resource = acquire_resource()
    use_resource(resource)
finally:
    if resource:
        resource.release()

# ✓ GOOD: Use context managers (better than finally)
with open('file.txt', 'r') as file:
    data = file.read()
# File automatically closed

# ✓ GOOD: Fail fast
def process(data):
    if not data:
        raise ValueError("Data required")
    # Process data

# ✗ AVOID: Catching too much
try:
    # Many operations
    operation1()
    operation2()
    operation3()
except Exception:
    # Which operation failed?
    pass

# ✓ GOOD: Narrow try blocks
try:
    operation1()
except SpecificError:
    handle_error()

try:
    operation2()
except AnotherError:
    handle_error()
```

---

## 18.6 Common Exception Types

### Built-in Exceptions

```python
# ValueError: Invalid value
try:
    int("abc")
except ValueError:
    print("Cannot convert to int")

# TypeError: Wrong type
try:
    "2" + 2
except TypeError:
    print("Cannot add string and int")

# KeyError: Key doesn't exist
try:
    d = {'a': 1}
    value = d['b']
except KeyError:
    print("Key not found")

# IndexError: Index out of range
try:
    lst = [1, 2, 3]
    value = lst[10]
except IndexError:
    print("Index out of range")

# FileNotFoundError: File doesn't exist
try:
    with open('nonexistent.txt') as f:
        data = f.read()
except FileNotFoundError:
    print("File not found")

# AttributeError: Attribute doesn't exist
try:
    x = 5
    x.append(1)
except AttributeError:
    print("Attribute doesn't exist")

# ZeroDivisionError: Division by zero
try:
    result = 10 / 0
except ZeroDivisionError:
    print("Cannot divide by zero")

# ImportError: Cannot import
try:
    import nonexistent_module
except ImportError:
    print("Module not found")

# RuntimeError: General runtime error
try:
    raise RuntimeError("Something went wrong")
except RuntimeError:
    print("Runtime error")
```

---

## 18.7 Context Managers

### with Statement

```python
# File handling
with open('file.txt', 'r') as file:
    data = file.read()
# File automatically closed

# Multiple resources
with open('input.txt', 'r') as infile, \
     open('output.txt', 'w') as outfile:
    data = infile.read()
    outfile.write(data)

# Custom context manager
class DatabaseConnection:
    def __enter__(self):
        print("Opening connection")
        self.conn = connect_to_database()
        return self.conn
    
    def __exit__(self, exc_type, exc_val, exc_tb):
        print("Closing connection")
        self.conn.close()
        return False  # Don't suppress exceptions

with DatabaseConnection() as conn:
    conn.execute("SELECT * FROM users")

# Using contextlib
from contextlib import contextmanager

@contextmanager
def managed_resource():
    resource = acquire_resource()
    try:
        yield resource
    finally:
        resource.release()

with managed_resource() as resource:
    use_resource(resource)
```

---

## 18.8 Assertion Errors

### Using Assertions

```python
# Basic assertion
x = 5
assert x > 0, "x must be positive"

# Development checks
def calculate_average(numbers):
    assert len(numbers) > 0, "List cannot be empty"
    return sum(numbers) / len(numbers)

# ✓ GOOD: Use for development checks
def set_age(age):
    assert isinstance(age, int), "Age must be int"
    assert age >= 0, "Age must be positive"
    self.age = age

# ✗ AVOID: For user input validation
def process_input(user_input):
    assert user_input  # BAD! Use proper validation
    return process(user_input)

# ✓ GOOD: Proper validation
def process_input(user_input):
    if not user_input:
        raise ValueError("Input required")
    return process(user_input)

# Note: Assertions can be disabled with -O flag
# python -O script.py  # Skips assertions
```

---

## 18.9 Logging Errors

### Error Logging

```python
import logging

# Configure logging
logging.basicConfig(
    level=logging.INFO,
    format='%(asctime)s - %(name)s - %(levelname)s - %(message)s'
)

logger = logging.getLogger(__name__)

# Log exceptions
try:
    risky_operation()
except Exception as e:
    logger.error(f"Operation failed: {e}")
    logger.exception("Full traceback:")  # Includes traceback

# Different log levels
try:
    operation()
except ValueError as e:
    logger.warning(f"Validation error: {e}")
except Exception as e:
    logger.error(f"Unexpected error: {e}")
    logger.exception("Details:")

# Custom exception logging
class CustomError(Exception):
    def __init__(self, message, code):
        super().__init__(message)
        self.code = code

try:
    raise CustomError("Failed", code=500)
except CustomError as e:
    logger.error(f"Error {e.code}: {e}")
```

---

## 18.10 Practice Problems

### Problem 1: Bare Except
```python
try:
    value = int(input())
except:
    print("Error")
```

<details>
<summary>Click for Answer</summary>

**Issue:** Catches everything, including KeyboardInterrupt

**Fix:**
```python
try:
    value = int(input())
except ValueError:  # ✓ Specific exception
    print("Invalid number")
except KeyboardInterrupt:
    print("Cancelled")
```
</details>

---

## 18.11 Key Takeaways

### What You Learned

1. **Use specific exceptions** - Not bare except
2. **Clean up in finally** - Or use context managers
3. **Don't suppress errors** - Log and re-raise
4. **Raise for validation** - Don't return None
5. **Create custom exceptions** - For app-specific errors
6. **Log exceptions** - Use logging.exception()
7. **Use with statement** - For resources

### Common Patterns

```python
# Pattern 1: Specific handling
try:
    operation()
except SpecificError:
    handle()

# Pattern 2: With cleanup
try:
    resource = acquire()
    use(resource)
finally:
    release(resource)

# Pattern 3: Context manager
with resource() as r:
    use(r)

# Pattern 4: Log and re-raise
try:
    operation()
except Exception as e:
    logger.error(f"Error: {e}")
    raise
```

---

## 18.12 Moving Forward

You now understand exception handling! In **Chapter 19**, we'll explore **Debugging Techniques**!

