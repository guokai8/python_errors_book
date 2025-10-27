# Chapter 14: NumPy - Array Computing Errors

## Introduction

Welcome to **NumPy** - the foundation of scientific computing in Python! NumPy provides powerful array operations and is the backbone of Pandas, SciPy, and most data science libraries.

Common errors:
- **ValueError**: Shape mismatches
- **IndexError**: Array index out of bounds
- **TypeError**: Wrong data types
- Broadcasting errors

Let's master NumPy!

---

## 14.1 Creating Arrays

### Basic Array Creation

```python
import numpy as np

# From list
arr = np.array([1, 2, 3, 4, 5])

# 2D array
arr_2d = np.array([[1, 2, 3], [4, 5, 6]])

# Zeros
zeros = np.zeros(5)          # [0. 0. 0. 0. 0.]
zeros_2d = np.zeros((3, 4))  # 3x4 matrix of zeros

# Ones
ones = np.ones(3)            # [1. 1. 1.]
ones_2d = np.ones((2, 3))    # 2x3 matrix of ones

# Range
arr = np.arange(0, 10, 2)    # [0 2 4 6 8]

# Linspace
arr = np.linspace(0, 1, 5)   # [0. 0.25 0.5 0.75 1.]

# Random
random = np.random.rand(5)    # 5 random numbers [0, 1)
random = np.random.randint(0, 10, 5)  # 5 random ints

# Identity matrix
identity = np.eye(3)          # 3x3 identity matrix

# Array info
print(arr.shape)    # Dimensions
print(arr.dtype)    # Data type
print(arr.size)     # Total elements
print(arr.ndim)     # Number of dimensions
```

---

### Error Type 1: `ValueError: setting an array element with a sequence`

**Error Message:**
```python
>>> import numpy as np
>>> arr = np.array([1, 2, [3, 4]])
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ValueError: setting an array element with a sequence.
```

**What Happened:**
Trying to create array with inconsistent dimensions.

**Why It Happens:**
- Nested lists of different lengths
- Mixed types in nested structure
- Jagged arrays

**Code Example - WRONG:**
```python
import numpy as np

# Inconsistent lengths
arr = np.array([[1, 2, 3], [4, 5]])  # ERROR! Different lengths

# Mixed nesting
arr = np.array([1, 2, [3, 4]])  # ERROR! Inconsistent depth

# Jagged array
data = [[1, 2], [3, 4, 5], [6]]
arr = np.array(data)  # ERROR or unexpected result
```

**Code Example - CORRECT:**
```python
import numpy as np

# Consistent dimensions
arr = np.array([[1, 2, 3], [4, 5, 6]])  # ✓ 2x3 array

# Same nesting level
arr = np.array([1, 2, 3, 4])  # ✓ 1D array
arr = np.array([[1, 2], [3, 4]])  # ✓ 2D array

# For jagged arrays, use dtype=object
data = [[1, 2], [3, 4, 5], [6]]
arr = np.array(data, dtype=object)  # ✓ Array of lists

# Or pad to same length
max_len = max(len(row) for row in data)
padded = [row + [0] * (max_len - len(row)) for row in data]
arr = np.array(padded)  # ✓

# Check before creating
data = [[1, 2, 3], [4, 5, 6]]
lengths = [len(row) for row in data]
if len(set(lengths)) == 1:
    arr = np.array(data)  # ✓ All same length
else:
    print("Inconsistent lengths")
```

---

## 14.2 Array Indexing and Slicing

### Accessing Elements

```python
import numpy as np

arr = np.array([10, 20, 30, 40, 50])

# Single element
print(arr[0])    # 10
print(arr[-1])   # 50

# Slicing
print(arr[1:4])  # [20 30 40]
print(arr[:3])   # [10 20 30]
print(arr[2:])   # [30 40 50]

# 2D array
arr_2d = np.array([[1, 2, 3], [4, 5, 6], [7, 8, 9]])

# Single element
print(arr_2d[0, 0])     # 1
print(arr_2d[1, 2])     # 6

# Row
print(arr_2d[0])        # [1 2 3]
print(arr_2d[0, :])     # [1 2 3]

# Column
print(arr_2d[:, 0])     # [1 4 7]

# Subarray
print(arr_2d[0:2, 1:3]) # [[2 3]
                        #  [5 6]]

# Boolean indexing
arr = np.array([1, 2, 3, 4, 5])
mask = arr > 2
print(arr[mask])        # [3 4 5]
print(arr[arr > 2])     # [3 4 5]

# Fancy indexing
indices = [0, 2, 4]
print(arr[indices])     # [1 3 5]
```

---

### Error Type 2: `IndexError: index out of bounds`

**Error Message:**
```python
>>> import numpy as np
>>> arr = np.array([1, 2, 3])
>>> print(arr[5])
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
IndexError: index 5 is out of bounds for axis 0 with size 3
```

**What Happened:**
Index exceeds array bounds.

**Code Example - WRONG:**
```python
import numpy as np

arr = np.array([1, 2, 3, 4, 5])

# Index too large
value = arr[10]  # ERROR! Only indices 0-4 exist

# Wrong dimension count
arr_2d = np.array([[1, 2], [3, 4]])
value = arr_2d[0, 5]  # ERROR! Column 5 doesn't exist

# Negative index too large
value = arr[-10]  # ERROR! Only -1 to -5 valid
```

**Code Example - CORRECT:**
```python
import numpy as np

arr = np.array([1, 2, 3, 4, 5])

# Check bounds
if index < len(arr):
    value = arr[index]  # ✓
else:
    print("Index out of bounds")

# Use .shape for multi-dimensional
arr_2d = np.array([[1, 2, 3], [4, 5, 6]])
rows, cols = arr_2d.shape
if row < rows and col < cols:
    value = arr_2d[row, col]  # ✓

# Use try/except
try:
    value = arr[index]
except IndexError:
    value = None  # ✓

# Safe indexing with clip
safe_index = np.clip(index, 0, len(arr) - 1)
value = arr[safe_index]  # ✓

# Use take with mode
value = arr.take(index, mode='clip')  # ✓ Clips to valid range
value = arr.take(index, mode='wrap')  # ✓ Wraps around
```

---

## 14.3 Array Operations

### Mathematical Operations

```python
import numpy as np

arr = np.array([1, 2, 3, 4, 5])

# Element-wise operations
print(arr + 10)      # [11 12 13 14 15]
print(arr * 2)       # [2 4 6 8 10]
print(arr ** 2)      # [1 4 9 16 25]

# Array operations
arr1 = np.array([1, 2, 3])
arr2 = np.array([4, 5, 6])
print(arr1 + arr2)   # [5 7 9]
print(arr1 * arr2)   # [4 10 18]

# Aggregations
print(arr.sum())     # 15
print(arr.mean())    # 3.0
print(arr.std())     # Standard deviation
print(arr.min())     # 1
print(arr.max())     # 5

# Along axis
arr_2d = np.array([[1, 2, 3], [4, 5, 6]])
print(arr_2d.sum(axis=0))  # [5 7 9] column sums
print(arr_2d.sum(axis=1))  # [6 15] row sums

# Universal functions
print(np.sqrt(arr))    # Square root
print(np.exp(arr))     # Exponential
print(np.log(arr))     # Natural log
print(np.sin(arr))     # Sine
```

---

### Error Type 3: `ValueError: operands could not be broadcast together`

**Error Message:**
```python
>>> import numpy as np
>>> arr1 = np.array([1, 2, 3])
>>> arr2 = np.array([1, 2])
>>> result = arr1 + arr2
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ValueError: operands could not be broadcast together with shapes (3,) (2,)
```

**What Happened:**
Trying to operate on arrays with incompatible shapes.

**Why It Happens:**
- Different array sizes
- Incompatible dimensions
- Wrong broadcasting

**Code Example - WRONG:**
```python
import numpy as np

# Different lengths
arr1 = np.array([1, 2, 3])
arr2 = np.array([1, 2])
result = arr1 + arr2  # ERROR! Shapes (3,) and (2,)

# Incompatible 2D shapes
arr1 = np.array([[1, 2, 3], [4, 5, 6]])  # 2x3
arr2 = np.array([[1, 2], [3, 4]])        # 2x2
result = arr1 + arr2  # ERROR! Incompatible

# Wrong dimension operations
arr1 = np.array([[1, 2], [3, 4]])  # 2x2
arr2 = np.array([1, 2, 3])         # 3 elements
result = arr1 + arr2  # ERROR! Can't broadcast
```

**Code Example - CORRECT:**
```python
import numpy as np

# Match array sizes
arr1 = np.array([1, 2, 3])
arr2 = np.array([1, 2, 3])  # ✓ Same size
result = arr1 + arr2

# Broadcasting with scalar
arr = np.array([1, 2, 3])
result = arr + 10  # ✓ Scalar broadcasts to all elements

# Broadcasting with compatible shapes
arr1 = np.array([[1, 2, 3], [4, 5, 6]])  # 2x3
arr2 = np.array([10, 20, 30])            # 3 elements
result = arr1 + arr2  # ✓ Broadcasts arr2 to each row
# [[11 22 33]
#  [14 25 36]]

# Reshape for broadcasting
arr1 = np.array([[1, 2], [3, 4]])  # 2x2
arr2 = np.array([10, 20])          # 2 elements
result = arr1 + arr2  # ✓ Broadcasts across columns

# Make compatible with reshape
arr1 = np.array([[1, 2], [3, 4]])    # 2x2
arr2 = np.array([10, 20])            # 2 elements
arr2_reshaped = arr2.reshape(2, 1)   # 2x1
result = arr1 + arr2_reshaped  # ✓
# [[11 12]
#  [23 24]]

# Check shapes before operation
if arr1.shape == arr2.shape:
    result = arr1 + arr2  # ✓
else:
    print(f"Incompatible: {arr1.shape} vs {arr2.shape}")

# Pad shorter array
arr1 = np.array([1, 2, 3])
arr2 = np.array([1, 2])
arr2_padded = np.pad(arr2, (0, len(arr1) - len(arr2)))  # ✓
result = arr1 + arr2_padded
```

**Broadcasting Rules:**
```python
# Arrays broadcast when:
# 1. They have same shape
# 2. One dimension is 1
# 3. One array has fewer dimensions

# Examples:
# (3, 4) + (3, 4)     ✓ Same shape
# (3, 4) + (4,)       ✓ Broadcasts to (3, 4)
# (3, 4) + (3, 1)     ✓ Broadcasts to (3, 4)
# (3, 4) + (1, 4)     ✓ Broadcasts to (3, 4)
# (3, 4) + (3, 5)     ✗ Incompatible
```

---

## 14.4 Reshaping Arrays

### Changing Array Shape

```python
import numpy as np

arr = np.array([1, 2, 3, 4, 5, 6])

# Reshape
arr_2d = arr.reshape(2, 3)  # 2x3
# [[1 2 3]
#  [4 5 6]]

# Flatten
arr_flat = arr_2d.flatten()  # [1 2 3 4 5 6]
arr_flat = arr_2d.ravel()    # Same but returns view

# Transpose
arr_t = arr_2d.T
# [[1 4]
#  [2 5]
#  [3 6]]

# Add dimension
arr_3d = arr[np.newaxis, :]    # (1, 6)
arr_3d = arr[:, np.newaxis]    # (6, 1)

# Squeeze (remove single dimensions)
arr = np.array([[[1, 2, 3]]])  # (1, 1, 3)
arr_squeezed = arr.squeeze()   # (3,)
```

---

### Error Type 4: `ValueError: cannot reshape array`

**Error Message:**
```python
>>> import numpy as np
>>> arr = np.array([1, 2, 3, 4, 5])
>>> arr.reshape(2, 3)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ValueError: cannot reshape array of size 5 into shape (2,3)
```

**What Happened:**
New shape doesn't match total number of elements.

**Code Example - WRONG:**
```python
import numpy as np

# Wrong total elements
arr = np.array([1, 2, 3, 4, 5])  # 5 elements
arr_2d = arr.reshape(2, 3)  # ERROR! 2*3=6 ≠ 5

# Incompatible shape
arr = np.array([1, 2, 3, 4])  # 4 elements
arr_2d = arr.reshape(3, 2)  # ERROR! 3*2=6 ≠ 4
```

**Code Example - CORRECT:**
```python
import numpy as np

# Matching total elements
arr = np.array([1, 2, 3, 4, 5, 6])  # 6 elements
arr_2d = arr.reshape(2, 3)  # ✓ 2*3=6
arr_2d = arr.reshape(3, 2)  # ✓ 3*2=6

# Use -1 to infer dimension
arr = np.array([1, 2, 3, 4, 5, 6])
arr_2d = arr.reshape(2, -1)  # ✓ Auto-calculates 3
arr_2d = arr.reshape(-1, 3)  # ✓ Auto-calculates 2

# Check if reshapeable
if arr.size % 3 == 0:
    arr_2d = arr.reshape(-1, 3)  # ✓
else:
    print("Cannot reshape to 3 columns")

# Pad to make reshapeable
arr = np.array([1, 2, 3, 4, 5])  # 5 elements
target_size = 6
arr_padded = np.pad(arr, (0, target_size - arr.size))  # ✓
arr_2d = arr_padded.reshape(2, 3)

# Use resize (changes size)
arr = np.array([1, 2, 3, 4, 5])
arr.resize((2, 3))  # ✓ Pads with zeros
# [[1 2 3]
#  [4 5 0]]
```

---

## 14.5 Common Patterns

### Useful Operations

```python
import numpy as np

# Stacking arrays
arr1 = np.array([1, 2, 3])
arr2 = np.array([4, 5, 6])
stacked_v = np.vstack([arr1, arr2])  # Vertical
# [[1 2 3]
#  [4 5 6]]
stacked_h = np.hstack([arr1, arr2])  # Horizontal
# [1 2 3 4 5 6]

# Where (conditional selection)
arr = np.array([1, 2, 3, 4, 5])
result = np.where(arr > 2, arr, 0)  # [0 0 3 4 5]

# Unique values
arr = np.array([1, 2, 2, 3, 3, 3])
unique = np.unique(arr)  # [1 2 3]

# Sorting
arr = np.array([3, 1, 4, 1, 5])
sorted_arr = np.sort(arr)  # [1 1 3 4 5]
indices = np.argsort(arr)  # [1 3 0 2 4]

# Finding elements
arr = np.array([1, 2, 3, 4, 5])
indices = np.where(arr > 3)  # (array([3, 4]),)
```

---

## 14.6 Practice Problems

### Problem 1: Shape Mismatch
```python
import numpy as np
arr1 = np.array([1, 2, 3])
arr2 = np.array([1, 2])
result = arr1 + arr2
```

<details>
<summary>Click for Answer</summary>

**Error:** `ValueError: operands could not be broadcast together`

**Fix:**
```python
import numpy as np
arr1 = np.array([1, 2, 3])
arr2 = np.array([1, 2, 3])  # ✓ Match sizes
result = arr1 + arr2
```
</details>

---

### Problem 2: Reshape Error
```python
import numpy as np
arr = np.array([1, 2, 3, 4, 5])
arr_2d = arr.reshape(2, 3)
```

<details>
<summary>Click for Answer</summary>

**Error:** `ValueError: cannot reshape array of size 5 into shape (2,3)`

**Fix:**
```python
import numpy as np
arr = np.array([1, 2, 3, 4, 5, 6])  # ✓ 6 elements
arr_2d = arr.reshape(2, 3)  # ✓ 2*3=6

# Or use -1
arr = np.array([1, 2, 3, 4, 5, 6])
arr_2d = arr.reshape(2, -1)  # ✓ Auto-calculates 3
```
</details>

---

## 14.7 Key Takeaways

### What You Learned

1. **Match array dimensions** - For operations
2. **Check shapes** - Before reshaping
3. **Use broadcasting** - For efficient operations
4. **Validate indices** - Before accessing
5. **Consistent nesting** - For array creation
6. **Use -1 in reshape** - To infer dimension
7. **Vectorize operations** - Avoid loops

---

## 14.8 Moving Forward

You now understand NumPy! In **Chapter 15**, we'll explore **Matplotlib** - data visualization!

