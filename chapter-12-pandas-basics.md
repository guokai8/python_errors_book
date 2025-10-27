# Chapter 12: Pandas Basics - DataFrame Errors

## Introduction

Welcome to **Pandas** - Python's powerful data analysis library! Pandas provides DataFrames for working with structured data (like spreadsheets or SQL tables). It's essential for data science and analysis.

Common errors:
- **KeyError**: Column/index doesn't exist
- **ValueError**: Wrong shape or values
- **AttributeError**: Wrong method for operation
- **TypeError**: Wrong data types
- Index alignment issues

Let's master Pandas!

---

## 12.1 Creating DataFrames

### Basic DataFrame Creation

```python
import pandas as pd

# From dictionary
data = {
    'name': ['Alice', 'Bob', 'Charlie'],
    'age': [25, 30, 35],
    'city': ['NYC', 'LA', 'Chicago']
}
df = pd.DataFrame(data)

# From list of lists
data = [
    ['Alice', 25, 'NYC'],
    ['Bob', 30, 'LA'],
    ['Charlie', 35, 'Chicago']
]
df = pd.DataFrame(data, columns=['name', 'age', 'city'])

# From list of dictionaries
data = [
    {'name': 'Alice', 'age': 25, 'city': 'NYC'},
    {'name': 'Bob', 'age': 30, 'city': 'LA'}
]
df = pd.DataFrame(data)

# Read from CSV
df = pd.read_csv('data.csv')

# Basic info
print(df.head())      # First 5 rows
print(df.tail())      # Last 5 rows
print(df.shape)       # (rows, columns)
print(df.columns)     # Column names
print(df.dtypes)      # Data types
print(df.info())      # Overview
```

---

### Error Type 1: `KeyError: 'column_name'`

**Error Message:**
```python
>>> import pandas as pd
>>> df = pd.DataFrame({'name': ['Alice', 'Bob'], 'age': [25, 30]})
>>> df['salary']
Traceback (most recent call last):
  ...
KeyError: 'salary'
```

**What Happened:**
Trying to access a column that doesn't exist.

**Why It Happens:**
- Column doesn't exist in DataFrame
- Typo in column name
- Case sensitivity
- Using wrong accessor

**Code Example - WRONG:**
```python
import pandas as pd

df = pd.DataFrame({
    'name': ['Alice', 'Bob'],
    'age': [25, 30]
})

# Non-existent column
salary = df['salary']  # ERROR! Column doesn't exist

# Typo
name = df['nane']  # ERROR! Typo

# Case sensitivity
age = df['Age']  # ERROR! Column is 'age' not 'Age'

# Wrong bracket type
name = df('name')  # ERROR! Use [] not ()

# Multiple columns with typo
subset = df[['name', 'salary']]  # ERROR! 'salary' doesn't exist
```

**Code Example - CORRECT:**
```python
import pandas as pd

df = pd.DataFrame({
    'name': ['Alice', 'Bob'],
    'age': [25, 30]
})

# Check if column exists
if 'salary' in df.columns:
    salary = df['salary']
else:
    print("Column doesn't exist")  # ✓

# Use .get() for Series (doesn't work for DataFrame columns)
# But can use try/except
try:
    salary = df['salary']
except KeyError:
    salary = None  # ✓

# Check available columns
print(df.columns.tolist())  # ['name', 'age']

# Correct spelling
name = df['name']  # ✓

# Match case exactly
age = df['age']  # ✓

# Use correct brackets
name = df['name']  # ✓

# Safe column selection
columns_to_select = ['name', 'age']
existing_cols = [col for col in columns_to_select if col in df.columns]
subset = df[existing_cols]  # ✓

# Add missing column with default
if 'salary' not in df.columns:
    df['salary'] = 0  # ✓ Add with default value

# Use .loc for safe access
try:
    data = df.loc[:, 'salary']
except KeyError:
    df['salary'] = 0
    data = df.loc[:, 'salary']  # ✓
```

---

## 12.2 Selecting Data

### Accessing Rows and Columns

```python
import pandas as pd

df = pd.DataFrame({
    'name': ['Alice', 'Bob', 'Charlie'],
    'age': [25, 30, 35],
    'city': ['NYC', 'LA', 'Chicago']
})

# Select column (returns Series)
ages = df['age']

# Select multiple columns (returns DataFrame)
subset = df[['name', 'age']]

# Select rows by index position (.iloc)
first_row = df.iloc[0]           # First row
first_three = df.iloc[0:3]       # First 3 rows
last_row = df.iloc[-1]           # Last row

# Select rows by label (.loc)
df_indexed = df.set_index('name')
alice = df_indexed.loc['Alice']

# Select specific cells
value = df.loc[0, 'age']         # Row 0, column 'age'
value = df.iloc[0, 1]            # Row 0, column 1

# Boolean indexing
adults = df[df['age'] >= 30]
in_nyc = df[df['city'] == 'NYC']

# Multiple conditions
result = df[(df['age'] >= 30) & (df['city'] == 'LA')]
```

---

### Error Type 2: `ValueError: Location based indexing can only have [integer, integer slice, listlike of integers, boolean array] types`

**Error Message:**
```python
>>> df = pd.DataFrame({'name': ['Alice', 'Bob'], 'age': [25, 30]})
>>> df.iloc['Alice']
Traceback (most recent call last):
  ...
ValueError: Location based indexing can only have [integer, integer slice...
```

**What Happened:**
Using wrong indexing method (.iloc vs .loc).

**Why It Happens:**
- Using labels with .iloc (needs integers)
- Using integers with .loc on non-integer index
- Confusing .iloc and .loc
- Wrong indexing syntax

**Code Example - WRONG:**
```python
import pandas as pd

df = pd.DataFrame({
    'name': ['Alice', 'Bob', 'Charlie'],
    'age': [25, 30, 35]
})

# Using label with .iloc
row = df.iloc['Alice']  # ERROR! .iloc needs integer

# Using column name with .iloc
ages = df.iloc[:, 'age']  # ERROR! Use column index or .loc

# Wrong syntax
row = df.iloc['name' == 'Alice']  # ERROR! Wrong method
```

**Code Example - CORRECT:**
```python
import pandas as pd

df = pd.DataFrame({
    'name': ['Alice', 'Bob', 'Charlie'],
    'age': [25, 30, 35]
})

# Use .iloc with integers
first_row = df.iloc[0]  # ✓ Integer index
first_three = df.iloc[0:3]  # ✓ Integer slice

# Use .loc with labels/conditions
# First, set index if you want to use labels
df_indexed = df.set_index('name')
alice = df_indexed.loc['Alice']  # ✓ Label index

# Or use .loc with column names
value = df.loc[0, 'age']  # ✓ Row by position, column by name

# Use .iloc for column by position
ages = df.iloc[:, 1]  # ✓ All rows, second column

# Boolean indexing (use direct or .loc)
adults = df[df['age'] >= 30]  # ✓
# Or
adults = df.loc[df['age'] >= 30]  # ✓

# Remember:
# .loc[row_label, column_label]  - uses labels
# .iloc[row_position, column_position]  - uses integers

# Examples:
df.loc[0, 'age']      # ✓ Row 0, column 'age'
df.iloc[0, 1]         # ✓ Row 0, column 1
df.loc[0:2, ['name', 'age']]  # ✓ Rows 0-2, specific columns
df.iloc[0:2, 0:2]     # ✓ First 2 rows, first 2 columns
```

---

## 12.3 Data Types

### Understanding dtypes

```python
import pandas as pd

# Check data types
df = pd.DataFrame({
    'name': ['Alice', 'Bob'],
    'age': [25, 30],
    'salary': [50000.0, 60000.0],
    'hired': ['2020-01-01', '2021-06-15']
})

print(df.dtypes)
# name       object
# age        int64
# salary     float64
# hired      object

# Convert types
df['age'] = df['age'].astype(float)
df['hired'] = pd.to_datetime(df['hired'])

# Check for missing values
print(df.isnull())
print(df.isnull().sum())  # Count per column

# Fill missing values
df['age'].fillna(0, inplace=True)
df['name'].fillna('Unknown', inplace=True)

# Drop missing values
df_clean = df.dropna()  # Drop rows with any NaN
df_clean = df.dropna(subset=['age'])  # Drop rows with NaN in 'age'
```

---

### Error Type 3: `TypeError: cannot concatenate object of type`

**Error Message:**
```python
>>> df = pd.DataFrame({'age': ['25', '30']})
>>> df['age'].mean()
Traceback (most recent call last):
  ...
TypeError: Could not convert 25 30 to numeric
```

**What Happened:**
Trying to perform numeric operations on non-numeric data.

**Why It Happens:**
- Column contains strings not numbers
- Mixed types in column
- Wrong data type
- Not converting before operation

**Code Example - WRONG:**
```python
import pandas as pd

# Numeric operations on strings
df = pd.DataFrame({'age': ['25', '30', '35']})
average = df['age'].mean()  # ERROR! Strings not numbers

# Mixed types
df = pd.DataFrame({'value': [1, 2, '3', 4]})
total = df['value'].sum()  # ERROR! Mixed types

# String operations on numbers
df = pd.DataFrame({'code': [101, 102, 103]})
upper = df['code'].str.upper()  # ERROR! Not strings
```

**Code Example - CORRECT:**
```python
import pandas as pd

# Convert to numeric first
df = pd.DataFrame({'age': ['25', '30', '35']})
df['age'] = pd.to_numeric(df['age'])  # ✓ Convert
average = df['age'].mean()  # ✓ Works now

# Handle errors in conversion
df = pd.DataFrame({'value': ['1', '2', 'invalid', '4']})
df['value'] = pd.to_numeric(df['value'], errors='coerce')  # ✓ NaN for invalid
# value: [1.0, 2.0, NaN, 4.0]

# Check dtype before operations
if pd.api.types.is_numeric_dtype(df['age']):
    average = df['age'].mean()  # ✓
else:
    print("Not numeric")

# Convert on creation
df = pd.DataFrame({
    'age': [25, 30, 35]  # ✓ Use numbers not strings
})

# Convert to string for string operations
df = pd.DataFrame({'code': [101, 102, 103]})
df['code'] = df['code'].astype(str)  # ✓ Convert to string
upper = df['code'].str.upper()  # ✓ Now works

# Specify dtypes when reading CSV
df = pd.read_csv('data.csv', dtype={'age': int, 'name': str})  # ✓

# Handle mixed types
df = pd.DataFrame({'value': [1, 2, '3', 4]})
df['value'] = df['value'].apply(lambda x: int(x) if isinstance(x, str) else x)  # ✓
```

---

## 12.4 Adding and Modifying Data

### Creating and Changing Columns

```python
import pandas as pd

df = pd.DataFrame({
    'name': ['Alice', 'Bob'],
    'age': [25, 30]
})

# Add new column
df['city'] = 'NYC'  # Same value for all
df['salary'] = [50000, 60000]  # Different values

# Create from calculation
df['age_in_months'] = df['age'] * 12

# Modify existing column
df['age'] = df['age'] + 1

# Conditional creation
df['is_adult'] = df['age'] >= 18

# Using .loc for modification
df.loc[df['age'] > 30, 'category'] = 'senior'
df.loc[df['age'] <= 30, 'category'] = 'junior'

# Apply function
df['name_upper'] = df['name'].apply(lambda x: x.upper())

# Rename columns
df.rename(columns={'age': 'years'}, inplace=True)

# Drop columns
df.drop('city', axis=1, inplace=True)
# Or
df = df.drop(columns=['city'])
```

---

### Error Type 4: `ValueError: Length of values does not match length of index`

**Error Message:**
```python
>>> df = pd.DataFrame({'name': ['Alice', 'Bob', 'Charlie']})
>>> df['age'] = [25, 30]
Traceback (most recent call last):
  ...
ValueError: Length of values (2) does not match length of index (3)
```

**What Happened:**
Trying to assign list with wrong length to column.

**Why It Happens:**
- List length doesn't match DataFrame rows
- Wrong number of values
- Off-by-one error

**Code Example - WRONG:**
```python
import pandas as pd

df = pd.DataFrame({
    'name': ['Alice', 'Bob', 'Charlie']
})

# Too few values
df['age'] = [25, 30]  # ERROR! 2 values, 3 rows

# Too many values
df['city'] = ['NYC', 'LA', 'Chicago', 'Boston']  # ERROR! 4 values, 3 rows
```

**Code Example - CORRECT:**
```python
import pandas as pd

df = pd.DataFrame({
    'name': ['Alice', 'Bob', 'Charlie']
})

# Match number of rows
df['age'] = [25, 30, 35]  # ✓ 3 values for 3 rows

# Use single value (broadcasts)
df['country'] = 'USA'  # ✓ Same value for all rows

# Check length first
ages = [25, 30]
if len(ages) == len(df):
    df['age'] = ages
else:
    print(f"Wrong length: need {len(df)}, got {len(ages)}")

# Pad with default if needed
ages = [25, 30]
while len(ages) < len(df):
    ages.append(0)  # Pad with 0
df['age'] = ages  # ✓

# Use .loc for conditional assignment
df['age'] = 0  # Initialize
df.loc[0, 'age'] = 25
df.loc[1, 'age'] = 30
df.loc[2, 'age'] = 35  # ✓

# Create from Series (index-aligned)
ages = pd.Series([25, 30, 35], index=[0, 1, 2])
df['age'] = ages  # ✓ Aligns by index
```

---

## 12.5 Filtering Data

### Boolean Indexing

```python
import pandas as pd

df = pd.DataFrame({
    'name': ['Alice', 'Bob', 'Charlie', 'David'],
    'age': [25, 30, 35, 40],
    'city': ['NYC', 'LA', 'NYC', 'Chicago']
})

# Single condition
adults_30plus = df[df['age'] >= 30]

# Multiple conditions (AND)
result = df[(df['age'] >= 30) & (df['city'] == 'NYC')]

# Multiple conditions (OR)
result = df[(df['age'] < 25) | (df['age'] > 35)]

# NOT condition
not_nyc = df[~(df['city'] == 'NYC')]
# Or
not_nyc = df[df['city'] != 'NYC']

# String contains
in_name = df[df['name'].str.contains('a', case=False)]

# isin() for multiple values
cities = df[df['city'].isin(['NYC', 'LA'])]

# Between
age_range = df[df['age'].between(25, 35)]

# Query method (alternative)
result = df.query('age >= 30 and city == "NYC"')
```

---

## 12.6 Common Operations

### Useful DataFrame Operations

```python
import pandas as pd

df = pd.DataFrame({
    'name': ['Alice', 'Bob', 'Charlie'],
    'age': [25, 30, 35],
    'salary': [50000, 60000, 70000]
})

# Sort
df_sorted = df.sort_values('age')
df_sorted = df.sort_values('age', ascending=False)
df_sorted = df.sort_values(['age', 'salary'])

# Group by
grouped = df.groupby('city')['salary'].mean()
grouped = df.groupby('city').agg({
    'age': 'mean',
    'salary': 'sum'
})

# Reset index
df_reset = df.reset_index(drop=True)

# Set index
df_indexed = df.set_index('name')

# Drop duplicates
df_unique = df.drop_duplicates()
df_unique = df.drop_duplicates(subset=['name'])

# Value counts
counts = df['city'].value_counts()

# Unique values
unique = df['city'].unique()

# Replace values
df['city'] = df['city'].replace('NYC', 'New York')

# Map values
city_map = {'NYC': 'New York', 'LA': 'Los Angeles'}
df['city_full'] = df['city'].map(city_map)
```

---

## 12.7 Practice Problems

### Problem 1: KeyError
```python
import pandas as pd
df = pd.DataFrame({'name': ['Alice'], 'age': [25]})
print(df['salary'])
```

<details>
<summary>Click for Answer</summary>

**Error:** `KeyError: 'salary'`

**Fix:**
```python
import pandas as pd
df = pd.DataFrame({'name': ['Alice'], 'age': [25]})

# Check first
if 'salary' in df.columns:
    print(df['salary'])
else:
    print("Column doesn't exist")  # ✓

# Or add with default
df['salary'] = 0
print(df['salary'])  # ✓
```
</details>

---

### Problem 2: Wrong Indexer
```python
import pandas as pd
df = pd.DataFrame({'name': ['Alice', 'Bob'], 'age': [25, 30]})
print(df.iloc[:, 'age'])
```

<details>
<summary>Click for Answer</summary>

**Error:** `ValueError: Location based indexing can only have...`

**Fix:**
```python
import pandas as pd
df = pd.DataFrame({'name': ['Alice', 'Bob'], 'age': [25, 30]})

# Use .loc for column names
print(df.loc[:, 'age'])  # ✓

# Or use .iloc with column position
print(df.iloc[:, 1])  # ✓

# Or direct column access
print(df['age'])  # ✓
```
</details>

---

### Problem 3: Length Mismatch
```python
import pandas as pd
df = pd.DataFrame({'name': ['Alice', 'Bob', 'Charlie']})
df['age'] = [25, 30]
```

<details>
<summary>Click for Answer</summary>

**Error:** `ValueError: Length of values does not match length of index`

**Fix:**
```python
import pandas as pd
df = pd.DataFrame({'name': ['Alice', 'Bob', 'Charlie']})

# Match number of rows
df['age'] = [25, 30, 35]  # ✓ 3 values

# Or use single value
df['age'] = 25  # ✓ Same for all
```
</details>

---

## 12.8 Key Takeaways

### What You Learned

1. **Check columns exist** - Use `in df.columns`
2. **Use .loc for labels** - .iloc for positions
3. **Convert data types** - pd.to_numeric(), .astype()
4. **Match lengths** - Values must match row count
5. **Use boolean indexing** - For filtering
6. **Handle missing values** - .fillna(), .dropna()
7. **Check dtypes** - Before operations

### Common Patterns

```python
# Pattern 1: Safe column access
if 'column' in df.columns:
    data = df['column']

# Pattern 2: Convert types
df['col'] = pd.to_numeric(df['col'], errors='coerce')

# Pattern 3: Filter data
filtered = df[df['age'] >= 30]

# Pattern 4: Add column safely
df['new'] = df['old'] * 2
```

### Error Summary

| Error | Cause | Prevention |
|-------|-------|------------|
| `KeyError` | Column doesn't exist | Check with `in df.columns` |
| `ValueError` (indexing) | Wrong indexer (.iloc vs .loc) | Use .loc for labels, .iloc for positions |
| `TypeError` | Wrong data type | Convert with pd.to_numeric() |
| `ValueError` (length) | Wrong number of values | Match DataFrame length |

---

## 12.9 Moving Forward

You now understand Pandas basics! In **Chapter 13**, we'll explore **Pandas Advanced** - merging, pivoting, and complex operations!

