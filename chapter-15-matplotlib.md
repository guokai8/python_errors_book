# Chapter 15: Matplotlib - Data Visualization Errors

## Introduction

Welcome to **Matplotlib** - Python's primary plotting library! Matplotlib creates publication-quality figures and is the foundation for many other visualization libraries.

Common errors:
- **ValueError**: Invalid data shapes
- **TypeError**: Wrong data types for plots
- **AttributeError**: Wrong method or property
- Figure/axis confusion

Let's master Matplotlib!

---

## 15.1 Basic Plotting

### Creating Simple Plots

```python
import matplotlib.pyplot as plt
import numpy as np

# Line plot
x = [1, 2, 3, 4, 5]
y = [2, 4, 6, 8, 10]
plt.plot(x, y)
plt.xlabel('X axis')
plt.ylabel('Y axis')
plt.title('Simple Line Plot')
plt.show()

# Multiple lines
x = np.linspace(0, 10, 100)
y1 = np.sin(x)
y2 = np.cos(x)
plt.plot(x, y1, label='sin(x)')
plt.plot(x, y2, label='cos(x)')
plt.legend()
plt.show()

# Scatter plot
x = [1, 2, 3, 4, 5]
y = [2, 3, 5, 7, 11]
plt.scatter(x, y)
plt.show()

# Bar plot
categories = ['A', 'B', 'C', 'D']
values = [4, 7, 2, 9]
plt.bar(categories, values)
plt.show()

# Histogram
data = np.random.randn(1000)
plt.hist(data, bins=30)
plt.show()
```

---

### Error Type 1: `ValueError: x and y must have same first dimension`

**Error Message:**
```python
>>> import matplotlib.pyplot as plt
>>> x = [1, 2, 3]
>>> y = [1, 2]
>>> plt.plot(x, y)
Traceback (most recent call last):
  ...
ValueError: x and y must have same first dimension, but have shapes (3,) and (2,)
```

**What Happened:**
X and Y arrays have different lengths.

**Why It Happens:**
- Different array sizes
- Data mismatch
- Missing values

**Code Example - WRONG:**
```python
import matplotlib.pyplot as plt

# Different lengths
x = [1, 2, 3, 4, 5]
y = [2, 4, 6]  # Only 3 values
plt.plot(x, y)  # ERROR! 5 vs 3

# Accidental truncation
x = range(10)
y = [i**2 for i in range(5)]  # Only 5 values
plt.plot(x, y)  # ERROR!
```

**Code Example - CORRECT:**
```python
import matplotlib.pyplot as plt
import numpy as np

# Match lengths
x = [1, 2, 3, 4, 5]
y = [2, 4, 6, 8, 10]  # ✓ 5 values
plt.plot(x, y)
plt.show()

# Check lengths before plotting
if len(x) == len(y):
    plt.plot(x, y)  # ✓
else:
    print(f"Length mismatch: {len(x)} vs {len(y)}")

# Generate matching arrays
x = np.linspace(0, 10, 100)
y = x ** 2  # ✓ Automatically same length
plt.plot(x, y)

# Truncate to shorter length
min_len = min(len(x), len(y))
plt.plot(x[:min_len], y[:min_len])  # ✓

# Fill missing values
x = [1, 2, 3, 4, 5]
y = [2, 4, 6]
while len(y) < len(x):
    y.append(0)  # ✓ Pad with zeros
plt.plot(x, y)
```

---

## 15.2 Subplots

### Multiple Plots

```python
import matplotlib.pyplot as plt
import numpy as np

# Create figure with subplots
fig, axes = plt.subplots(2, 2, figsize=(10, 8))

x = np.linspace(0, 10, 100)

# Access subplots
axes[0, 0].plot(x, np.sin(x))
axes[0, 0].set_title('Sine')

axes[0, 1].plot(x, np.cos(x))
axes[0, 1].set_title('Cosine')

axes[1, 0].plot(x, x**2)
axes[1, 0].set_title('Square')

axes[1, 1].plot(x, np.exp(x/10))
axes[1, 1].set_title('Exponential')

plt.tight_layout()
plt.show()

# Single row
fig, axes = plt.subplots(1, 3, figsize=(15, 5))
for i, ax in enumerate(axes):
    ax.plot(x, x**i)
    ax.set_title(f'x^{i}')
plt.show()
```

---

### Error Type 2: `AttributeError: 'numpy.ndarray' object has no attribute 'plot'`

**Error Message:**
```python
>>> fig, axes = plt.subplots(2, 2)
>>> axes.plot([1, 2, 3])
Traceback (most recent call last):
  ...
AttributeError: 'numpy.ndarray' object has no attribute 'plot'
```

**What Happened:**
Calling plot() on axes array instead of individual axis.

**Why It Happens:**
- Confusing axes array with single axis
- Wrong indexing
- Not understanding subplots return type

**Code Example - WRONG:**
```python
import matplotlib.pyplot as plt

# Multiple subplots - axes is array
fig, axes = plt.subplots(2, 2)
axes.plot([1, 2, 3])  # ERROR! axes is array

# Wrong method on figure
fig = plt.figure()
fig.plot([1, 2, 3])  # ERROR! Use ax, not fig
```

**Code Example - CORRECT:**
```python
import matplotlib.pyplot as plt
import numpy as np

# Single subplot - axes is single axis
fig, ax = plt.subplots()
ax.plot([1, 2, 3])  # ✓ ax is single axis
plt.show()

# Multiple subplots - index into array
fig, axes = plt.subplots(2, 2)
axes[0, 0].plot([1, 2, 3])  # ✓ Index specific axis
axes[0, 1].plot([1, 4, 9])  # ✓
plt.show()

# Flatten for easy iteration
fig, axes = plt.subplots(2, 2)
axes_flat = axes.flatten()
for i, ax in enumerate(axes_flat):
    ax.plot([1, 2, 3])  # ✓
plt.show()

# Use plt.subplot (different approach)
plt.subplot(2, 2, 1)
plt.plot([1, 2, 3])  # ✓
plt.subplot(2, 2, 2)
plt.plot([1, 4, 9])  # ✓
plt.show()

# Check type
fig, axes = plt.subplots(2, 2)
if isinstance(axes, np.ndarray):
    for ax in axes.flat:
        ax.plot([1, 2, 3])  # ✓
else:
    axes.plot([1, 2, 3])  # ✓
```

---

## 15.3 Customizing Plots

### Styling and Formatting

```python
import matplotlib.pyplot as plt
import numpy as np

x = np.linspace(0, 10, 100)
y = np.sin(x)

# Line style and color
plt.plot(x, y, 
         color='red',       # or 'r', '#FF0000', (1, 0, 0)
         linestyle='--',    # or ':', '-.', '-'
         linewidth=2,
         marker='o',
         markersize=5,
         label='sin(x)')

# Grid
plt.grid(True, alpha=0.3)

# Limits
plt.xlim(0, 10)
plt.ylim(-1.5, 1.5)

# Labels
plt.xlabel('Time', fontsize=12)
plt.ylabel('Value', fontsize=12)
plt.title('Sine Wave', fontsize=14, fontweight='bold')

# Legend
plt.legend(loc='upper right')

# Save figure
plt.savefig('plot.png', dpi=300, bbox_inches='tight')

plt.show()
```

---

## 15.4 Different Plot Types

### Common Visualizations

```python
import matplotlib.pyplot as plt
import numpy as np

# Scatter with colors
x = np.random.rand(50)
y = np.random.rand(50)
colors = np.random.rand(50)
sizes = 1000 * np.random.rand(50)
plt.scatter(x, y, c=colors, s=sizes, alpha=0.5, cmap='viridis')
plt.colorbar()
plt.show()

# Bar plot
categories = ['A', 'B', 'C', 'D', 'E']
values = [23, 45, 56, 78, 32]
plt.bar(categories, values, color='steelblue')
plt.xticks(rotation=45)
plt.show()

# Horizontal bar
plt.barh(categories, values, color='coral')
plt.show()

# Histogram
data = np.random.randn(1000)
plt.hist(data, bins=30, color='skyblue', edgecolor='black')
plt.xlabel('Value')
plt.ylabel('Frequency')
plt.show()

# Box plot
data = [np.random.normal(0, std, 100) for std in range(1, 4)]
plt.boxplot(data, labels=['Group 1', 'Group 2', 'Group 3'])
plt.show()

# Pie chart
sizes = [30, 25, 20, 25]
labels = ['A', 'B', 'C', 'D']
plt.pie(sizes, labels=labels, autopct='%1.1f%%', startangle=90)
plt.axis('equal')
plt.show()

# Heatmap
data = np.random.rand(10, 10)
plt.imshow(data, cmap='hot', interpolation='nearest')
plt.colorbar()
plt.show()
```

---

### Error Type 3: `TypeError: Image data of dtype object cannot be converted to float`

**What Happened:**
Wrong data type for plot.

**Code Example - WRONG:**
```python
import matplotlib.pyplot as plt

# String data for numerical plot
data = ['a', 'b', 'c']
plt.plot(data)  # ERROR! Can't plot strings

# Mixed types
x = [1, 2, '3', 4]
y = [1, 4, 9, 16]
plt.plot(x, y)  # ERROR! Mixed types
```

**Code Example - CORRECT:**
```python
import matplotlib.pyplot as plt
import numpy as np

# Convert to numbers
data = ['1', '2', '3', '4']
data_numeric = [float(x) for x in data]  # ✓
plt.plot(data_numeric)

# Use categorical plot for strings
categories = ['A', 'B', 'C', 'D']
values = [1, 3, 2, 4]
plt.bar(categories, values)  # ✓

# Handle missing/invalid data
data = [1, 2, None, 4, 5]
data_clean = [x for x in data if x is not None]  # ✓
plt.plot(data_clean)

# Use pandas for automatic handling
import pandas as pd
df = pd.DataFrame({'x': [1, 2, 3], 'y': [1, 4, 9]})
df.plot(x='x', y='y')  # ✓
```

---

## 15.5 Common Patterns

### Useful Techniques

```python
import matplotlib.pyplot as plt
import numpy as np

# Multiple y-axes
fig, ax1 = plt.subplots()
ax2 = ax1.twinx()

x = np.linspace(0, 10, 100)
ax1.plot(x, np.sin(x), 'b-')
ax2.plot(x, x**2, 'r-')
ax1.set_ylabel('sin(x)', color='b')
ax2.set_ylabel('x^2', color='r')

# Annotations
plt.plot([1, 2, 3, 4], [1, 4, 9, 16])
plt.annotate('Peak', xy=(4, 16), xytext=(3, 12),
             arrowprops=dict(arrowstyle='->'))

# Fill between
x = np.linspace(0, 10, 100)
y1 = np.sin(x)
y2 = np.cos(x)
plt.plot(x, y1)
plt.plot(x, y2)
plt.fill_between(x, y1, y2, alpha=0.3)

# Log scale
plt.plot(x, np.exp(x))
plt.yscale('log')

# Style sheets
plt.style.use('seaborn')  # or 'ggplot', 'dark_background'
```

---

## 15.6 Practice Problems

### Problem 1: Length Mismatch
```python
import matplotlib.pyplot as plt
x = [1, 2, 3, 4, 5]
y = [1, 4, 9]
plt.plot(x, y)
```

<details>
<summary>Click for Answer</summary>

**Error:** `ValueError: x and y must have same first dimension`

**Fix:**
```python
import matplotlib.pyplot as plt
x = [1, 2, 3, 4, 5]
y = [1, 4, 9, 16, 25]  # ✓ Match length
plt.plot(x, y)
plt.show()
```
</details>

---

### Problem 2: Wrong Axes Access
```python
import matplotlib.pyplot as plt
fig, axes = plt.subplots(2, 2)
axes.plot([1, 2, 3])
```

<details>
<summary>Click for Answer</summary>

**Error:** `AttributeError: 'numpy.ndarray' object has no attribute 'plot'`

**Fix:**
```python
import matplotlib.pyplot as plt
fig, axes = plt.subplots(2, 2)
axes[0, 0].plot([1, 2, 3])  # ✓ Index specific axis
plt.show()
```
</details>

---

## 15.7 Key Takeaways

### What You Learned

1. **Match array lengths** - X and Y must be same size
2. **Index subplot axes** - axes[i, j] for multiple plots
3. **Use ax methods** - Not plt when using subplots
4. **Check data types** - Convert strings to numbers
5. **Use tight_layout()** - Prevent overlapping
6. **Save before show()** - Or figure won't save
7. **Close figures** - plt.close() to free memory

### Common Patterns

```python
# Pattern 1: Basic plot
plt.plot(x, y)
plt.xlabel('X')
plt.ylabel('Y')
plt.title('Title')
plt.show()

# Pattern 2: Subplots
fig, axes = plt.subplots(2, 2)
axes[0, 0].plot(x, y)
plt.tight_layout()
plt.show()

# Pattern 3: Save figure
plt.plot(x, y)
plt.savefig('plot.png', dpi=300)
plt.show()
```

### Error Summary

| Error | Cause | Prevention |
|-------|-------|------------|
| `ValueError` (dimension) | X and Y different lengths | Match array sizes |
| `AttributeError` | Wrong axes access | Index axes array properly |
| `TypeError` | Wrong data type | Convert to numeric |

---

## 15.8 Congratulations - Part II Complete!

### 🎉 You Completed Part II: Libraries and Data!

You've mastered:
- ✅ Regular Expressions (Chapter 11)
- ✅ Pandas Basics (Chapter 12)
- ✅ Pandas Advanced (Chapter 13)
- ✅ NumPy (Chapter 14)
- ✅ Matplotlib (Chapter 15)

**Total Progress: 15/20 chapters (75%) complete!**

---

## 15.9 Moving Forward

**What's Next: Part III - Advanced Topics (Chapters 16-20)**
- Chapter 16: Object-Oriented Programming
- Chapter 17: Modules and Imports
- Chapter 18: Exception Handling
- Chapter 19: Debugging Techniques
- Chapter 20: Testing and Code Quality
