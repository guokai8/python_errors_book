# Chapter 13: Pandas Advanced - Complex Operations

## Introduction

You've learned Pandas basics. Now let's explore **advanced Pandas** - merging, joining, pivoting, grouping, and complex data transformations. These skills are essential for real-world data analysis.

Common errors:
- **MergeError**: Wrong merge keys
- **ValueError**: Shape mismatches
- **KeyError**: Index/column issues
- Memory errors with large datasets

Let's master advanced Pandas!

---

## 13.1 Merging DataFrames

### Combining DataFrames

```python
import pandas as pd

# Sample data
df1 = pd.DataFrame({
    'id': [1, 2, 3],
    'name': ['Alice', 'Bob', 'Charlie']
})

df2 = pd.DataFrame({
    'id': [1, 2, 4],
    'salary': [50000, 60000, 70000]
})

# Inner join (default)
merged = pd.merge(df1, df2, on='id')
# Only keeps matching rows (id 1, 2)

# Left join
merged = pd.merge(df1, df2, on='id', how='left')
# Keeps all df1 rows, fills NaN for missing df2

# Right join
merged = pd.merge(df1, df2, on='id', how='right')
# Keeps all df2 rows

# Outer join
merged = pd.merge(df1, df2, on='id', how='outer')
# Keeps all rows from both

# Merge on different column names
df1 = pd.DataFrame({'emp_id': [1, 2], 'name': ['Alice', 'Bob']})
df2 = pd.DataFrame({'employee_id': [1, 2], 'salary': [50000, 60000]})
merged = pd.merge(df1, df2, left_on='emp_id', right_on='employee_id')

# Merge on index
merged = pd.merge(df1, df2, left_index=True, right_index=True)
```

---

### Error Type 1: `MergeError` or Wrong Results

**What Happened:**
Merge produces unexpected results or errors.

**Why It Happens:**
- Wrong merge key
- Duplicate keys
- Missing values in key columns
- Wrong merge type

**Code Example - WRONG:**
```python
import pandas as pd

df1 = pd.DataFrame({
    'id': [1, 2, 3],
    'name': ['Alice', 'Bob', 'Charlie']
})

df2 = pd.DataFrame({
    'employee_id': [1, 2, 4],  # Different column name!
    'salary': [50000, 60000, 70000]
})

# Wrong: merging on non-existent 'id' in df2
merged = pd.merge(df1, df2, on='id')  # ERROR or empty result

# Duplicate keys without handling
df1 = pd.DataFrame({
    'id': [1, 1, 2],  # Duplicate id=1
    'name': ['Alice', 'Alice2', 'Bob']
})
df2 = pd.DataFrame({
    'id': [1, 1, 2],  # Duplicate id=1
    'salary': [50000, 55000, 60000]
})
merged = pd.merge(df1, df2, on='id')  # Creates cartesian product!
```

**Code Example - CORRECT:**
```python
import pandas as pd

df1 = pd.DataFrame({
    'id': [1, 2, 3],
    'name': ['Alice', 'Bob', 'Charlie']
})

df2 = pd.DataFrame({
    'employee_id': [1, 2, 4],
    'salary': [50000, 60000, 70000]
})

# Use left_on and right_on for different names
merged = pd.merge(df1, df2, 
                  left_on='id', 
                  right_on='employee_id')  # ✓

# Check for duplicates before merging
print("Duplicates in df1:", df1['id'].duplicated().sum())
print("Duplicates in df2:", df2['employee_id'].duplicated().sum())

# Remove duplicates if needed
df1 = df1.drop_duplicates(subset=['id'])  # ✓

# Specify how to handle many-to-many
merged = pd.merge(df1, df2, on='id', how='left', validate='1:1')  # ✓
# validate options: '1:1', '1:m', 'm:1', 'm:m'

# Check result
print(f"df1 rows: {len(df1)}, df2 rows: {len(df2)}, merged rows: {len(merged)}")

# Handle missing keys
merged = pd.merge(df1, df2, on='id', how='outer', indicator=True)  # ✓
# indicator shows where each row came from
```

---

## 13.2 Concatenating DataFrames

### Stacking DataFrames

```python
import pandas as pd

df1 = pd.DataFrame({
    'name': ['Alice', 'Bob'],
    'age': [25, 30]
})

df2 = pd.DataFrame({
    'name': ['Charlie', 'David'],
    'age': [35, 40]
})

# Concatenate vertically (stack rows)
combined = pd.concat([df1, df2], ignore_index=True)

# Concatenate horizontally (side by side)
combined = pd.concat([df1, df2], axis=1)

# With keys to identify source
combined = pd.concat([df1, df2], keys=['first', 'second'])

# Only keep common columns
combined = pd.concat([df1, df2], join='inner')
```

---

### Error Type 2: `ValueError: Shape mismatch` in concat

**What Happened:**
Concatenating DataFrames with incompatible shapes.

**Code Example - WRONG:**
```python
import pandas as pd

df1 = pd.DataFrame({
    'A': [1, 2, 3],
    'B': [4, 5, 6]
})

df2 = pd.DataFrame({
    'A': [7, 8],  # Only 2 rows!
    'C': [9, 10]  # Different column!
})

# Horizontal concat with different row counts
combined = pd.concat([df1, df2], axis=1)  # Fills NaN but might be unexpected
```

**Code Example - CORRECT:**
```python
import pandas as pd

df1 = pd.DataFrame({
    'A': [1, 2, 3],
    'B': [4, 5, 6]
})

df2 = pd.DataFrame({
    'A': [7, 8],
    'C': [9, 10]
})

# Vertical concat (rows) - works with different columns
combined = pd.concat([df1, df2], ignore_index=True)  # ✓
# Fills NaN for missing columns

# Check shapes before concat
print(f"df1: {df1.shape}, df2: {df2.shape}")

# Reset index if needed
df1_reset = df1.reset_index(drop=True)
df2_reset = df2.reset_index(drop=True)
combined = pd.concat([df1_reset, df2_reset], ignore_index=True)  # ✓

# Only keep matching columns for horizontal concat
common_cols = list(set(df1.columns) & set(df2.columns))
if common_cols:
    combined = pd.concat([df1[common_cols], df2[common_cols]], axis=1)  # ✓

# Use merge instead if you have a key
combined = pd.merge(df1, df2, on='A', how='outer')  # ✓
```

---

## 13.3 Pivot Tables

### Reshaping Data

```python
import pandas as pd

df = pd.DataFrame({
    'date': ['2024-01', '2024-01', '2024-02', '2024-02'],
    'city': ['NYC', 'LA', 'NYC', 'LA'],
    'sales': [100, 150, 200, 175]
})

# Create pivot table
pivot = df.pivot_table(
    values='sales',
    index='date',
    columns='city',
    aggfunc='sum'
)
#         LA   NYC
# 2024-01 150  100
# 2024-02 175  200

# Multiple aggregations
pivot = df.pivot_table(
    values='sales',
    index='date',
    columns='city',
    aggfunc=['sum', 'mean', 'count']
)

# Fill missing values
pivot = df.pivot_table(
    values='sales',
    index='date',
    columns='city',
    aggfunc='sum',
    fill_value=0
)

# Melt (reverse of pivot)
melted = pivot.reset_index().melt(
    id_vars=['date'],
    value_vars=['NYC', 'LA'],
    var_name='city',
    value_name='sales'
)
```

---

## 13.4 GroupBy Operations

### Aggregating Data

```python
import pandas as pd

df = pd.DataFrame({
    'city': ['NYC', 'NYC', 'LA', 'LA', 'Chicago'],
    'year': [2023, 2024, 2023, 2024, 2023],
    'sales': [100, 150, 200, 175, 90]
})

# Simple groupby
grouped = df.groupby('city')['sales'].sum()

# Multiple columns
grouped = df.groupby(['city', 'year'])['sales'].sum()

# Multiple aggregations
grouped = df.groupby('city').agg({
    'sales': ['sum', 'mean', 'count']
})

# Custom aggregation
grouped = df.groupby('city')['sales'].agg(
    total='sum',
    average='mean',
    maximum='max'
)

# Transform (keep original shape)
df['sales_pct'] = df.groupby('city')['sales'].transform(
    lambda x: x / x.sum() * 100
)

# Filter groups
filtered = df.groupby('city').filter(
    lambda x: x['sales'].sum() > 200
)

# Apply custom function
def custom_func(group):
    return group['sales'].max() - group['sales'].min()

result = df.groupby('city').apply(custom_func)
```

---

### Error Type 3: `KeyError` in GroupBy

**What Happened:**
Column doesn't exist in grouped result.

**Code Example - WRONG:**
```python
import pandas as pd

df = pd.DataFrame({
    'city': ['NYC', 'LA'],
    'sales': [100, 200]
})

# Groupby returns Series not DataFrame
grouped = df.groupby('city')['sales'].sum()
# Now grouped is a Series

# Trying to access like DataFrame
result = grouped['city']  # ERROR! Series doesn't have 'city' column
```

**Code Example - CORRECT:**
```python
import pandas as pd

df = pd.DataFrame({
    'city': ['NYC', 'LA'],
    'sales': [100, 200]
})

# Keep as DataFrame
grouped = df.groupby('city')[['sales']].sum()  # ✓ Double brackets
# Or
grouped = df.groupby('city').sum()  # ✓

# Reset index to access groupby column
grouped = df.groupby('city')['sales'].sum().reset_index()  # ✓
print(grouped['city'])  # ✓ Now works

# Access index directly
grouped = df.groupby('city')['sales'].sum()
cities = grouped.index  # ✓ Get city names from index

# Use .agg() for DataFrame result
grouped = df.groupby('city').agg({'sales': 'sum'})  # ✓
```

---

## 13.5 Working with Dates

### DateTime Operations

```python
import pandas as pd

# Create datetime column
df = pd.DataFrame({
    'date_str': ['2024-01-15', '2024-02-20', '2024-03-10']
})

# Convert to datetime
df['date'] = pd.to_datetime(df['date_str'])

# Extract components
df['year'] = df['date'].dt.year
df['month'] = df['date'].dt.month
df['day'] = df['date'].dt.day
df['day_of_week'] = df['date'].dt.day_name()
df['quarter'] = df['date'].dt.quarter

# Date arithmetic
df['next_week'] = df['date'] + pd.Timedelta(days=7)
df['last_month'] = df['date'] - pd.DateOffset(months=1)

# Date range
dates = pd.date_range('2024-01-01', '2024-12-31', freq='D')

# Resample time series
df = df.set_index('date')
monthly = df.resample('M').sum()

# Rolling windows
df['rolling_mean'] = df['sales'].rolling(window=7).mean()
```

---

## 13.6 Apply and Transform

### Custom Functions

```python
import pandas as pd

df = pd.DataFrame({
    'name': ['alice', 'bob', 'charlie'],
    'age': [25, 30, 35]
})

# Apply to column (Series)
df['name_upper'] = df['name'].apply(lambda x: x.upper())

# Apply to multiple columns
df['age_category'] = df['age'].apply(
    lambda x: 'young' if x < 30 else 'old'
)

# Apply to DataFrame (row-wise)
def categorize(row):
    if row['age'] < 30:
        return 'junior'
    return 'senior'

df['category'] = df.apply(categorize, axis=1)

# Apply element-wise (applymap) - deprecated, use .map()
df_numeric = df[['age']]
df_numeric = df_numeric.map(lambda x: x * 2)

# Vectorized operations (FASTER)
df['age_doubled'] = df['age'] * 2  # ✓ Better than apply
df['is_adult'] = df['age'] >= 18   # ✓ Vectorized
```

---

## 13.7 Memory Optimization

### Handling Large DataFrames

```python
import pandas as pd

# Read in chunks
chunk_size = 10000
chunks = []
for chunk in pd.read_csv('large_file.csv', chunksize=chunk_size):
    # Process chunk
    processed = chunk[chunk['age'] > 18]
    chunks.append(processed)

df = pd.concat(chunks, ignore_index=True)

# Optimize dtypes
df['age'] = df['age'].astype('int8')  # Instead of int64
df['category'] = df['category'].astype('category')

# Check memory usage
print(df.memory_usage(deep=True))
print(df.info(memory_usage='deep'))

# Use appropriate dtypes when reading
df = pd.read_csv('data.csv', dtype={
    'age': 'int8',
    'category': 'category'
})

# Select columns
df = pd.read_csv('data.csv', usecols=['name', 'age'])
```

---

## 13.8 Practice Problems

### Problem 1: Merge Error
```python
import pandas as pd
df1 = pd.DataFrame({'id': [1, 2], 'name': ['Alice', 'Bob']})
df2 = pd.DataFrame({'emp_id': [1, 2], 'salary': [50000, 60000]})
merged = pd.merge(df1, df2, on='id')
```

<details>
<summary>Click for Answer</summary>

**Issue:** Column 'id' doesn't exist in df2

**Fix:**
```python
import pandas as pd
df1 = pd.DataFrame({'id': [1, 2], 'name': ['Alice', 'Bob']})
df2 = pd.DataFrame({'emp_id': [1, 2], 'salary': [50000, 60000]})

# Use left_on and right_on
merged = pd.merge(df1, df2, left_on='id', right_on='emp_id')  # ✓
```
</details>

---

### Problem 2: GroupBy KeyError
```python
import pandas as pd
df = pd.DataFrame({'city': ['NYC', 'LA'], 'sales': [100, 200]})
grouped = df.groupby('city')['sales'].sum()
print(grouped['city'])
```

<details>
<summary>Click for Answer</summary>

**Error:** `KeyError: 'city'`

**Why:** GroupBy result is Series, 'city' is index

**Fix:**
```python
import pandas as pd
df = pd.DataFrame({'city': ['NYC', 'LA'], 'sales': [100, 200]})
grouped = df.groupby('city')['sales'].sum()

# Reset index
grouped = grouped.reset_index()  # ✓
print(grouped['city'])  # ✓ Works now

# Or access index
cities = grouped.index  # ✓
```
</details>

---

## 13.9 Key Takeaways

### What You Learned

1. **Specify merge keys** - Use left_on/right_on
2. **Check for duplicates** - Before merging
3. **Use ignore_index** - When concatenating
4. **Reset index** - After groupby to access columns
5. **Optimize dtypes** - For memory efficiency
6. **Use vectorized ops** - Faster than apply
7. **Process in chunks** - For large files

### Common Patterns

```python
# Pattern 1: Safe merge
merged = pd.merge(df1, df2, 
                  left_on='id1', 
                  right_on='id2',
                  how='left')

# Pattern 2: Concat with reset
combined = pd.concat([df1, df2], ignore_index=True)

# Pattern 3: GroupBy with reset
result = df.groupby('col')['val'].sum().reset_index()

# Pattern 4: Vectorized operations
df['new'] = df['old'] * 2  # Better than apply
```

---

## 13.10 Moving Forward

You now understand advanced Pandas! In **Chapter 14**, we'll explore **NumPy** - numerical computing!
