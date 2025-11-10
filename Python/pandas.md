# Pandas: DataFrame Operations Reference

## Table of Contents
1. [Introduction](#introduction)
2. [DataFrame Creation and Inspection](#dataframe-creation-and-inspection)
3. [Data Selection and Filtering](#data-selection-and-filtering)
4. [Data Transformation](#data-transformation)
5. [Data Aggregation](#data-aggregation)
6. [Data Cleaning](#data-cleaning)
7. [Data Reshaping](#data-reshaping)
8. [Export and Import](#export-and-import)
9. [Visualization](#visualization)

---

## Introduction

Pandas is a powerful data manipulation and analysis library for Python. This guide provides a comprehensive reference for common DataFrame operations, organized by functionality.

**Key Concepts:**
- **DataFrame**: Two-dimensional labeled data structure (like a spreadsheet)
- **Series**: One-dimensional labeled array
- **Index**: Row labels
- **Columns**: Column labels

---

## DataFrame Creation and Inspection

### Basic Information

**Get data types:**

```python
df.dtypes
```

**Get comprehensive information:**

```python
df.info()
```

**Check for null values:**

```python
df.isnull().sum()
```

**Get number of unique values:**

```python
df.nunique()  # Return number of unique elements in the object
```

**Get value counts:**

```python
df['feature'].value_counts()
```

### Copying DataFrames

**Create an independent copy:**

```python
df_new = df_old.copy()  # Creates a new DataFrame
# Changes to df_new won't affect df_old
```

**Important:** Without `.copy()`, both variables reference the same DataFrame, so changes affect both.

---

## Data Selection and Filtering

### Column Selection

**Select specific columns:**

```python
df[['id', 'desc']]  # Select multiple columns
```

**Select by data type:**

```python
obj = data.select_dtypes(include=['object'])
```

### Row Selection

**Select rows by index:**

```python
df[210:220]  # Select rows by range
df.iloc[[2]]  # Select single row by position
df.iloc[0:3, 1:]  # iloc[row slicing, column slicing]
```

**Select rows by label:**

```python
data.ix[row name slicing, column name slicing]  # Note: deprecated in newer pandas
```

**Select rows by condition:**

```python
# Single condition
df[df['id'] == 12290]

# Multiple conditions with AND
df[(df['std'] > 1000) & (df['msg_qty'] == 10)]

# Multiple conditions with OR
df[(df.tourist == 1) | (df.borders == 1) | (df.travel == 1)]

# Using between
df[df['age'].between(15, 25, inclusive=True)]
```

**Select rows by index membership:**

```python
# Select rows where index is in a list
df[df.index.isin(kill)]

# Select rows where index is NOT in a list
df[~df.index.isin(kill)]
```

**Get index values:**

```python
df[df['id'] == 12290].index.get_values()  # Get index values
```

### Reset Index

**Reset index to default integer index:**

```python
df.reset_index(drop=True)  # drop=True removes old index
```

---

## Data Transformation

### Type Conversion

**Convert column to specific type:**

```python
df['feature'].astype('int64')
```

**Convert to NumPy array:**

```python
df.values  # Returns numpy ndarray
df.as_matrix()  # Returns numpy ndarray (deprecated, use .values)
```

### String Operations

**Replace characters using regex:**

```python
df['FullDescription'].replace('[^a-zA-Z0-9]', ' ', regex=True)
```

**Split strings:**

```python
data['col_4'] = data['col_4'].str.split().apply(lambda x: x[0])
```

**Convert to datetime:**

```python
data['col_6'] = pd.to_datetime(data['col_6'], format='%d.%m.%Y', errors='coerce')
```

### Applying Functions

**Apply function to DataFrame:**

```python
DataFrame.apply(func, axis=0)
# axis=0: apply to each column
# axis=1: apply to each row, returns Series
```

**Map values:**

```python
df['Sex'].map(lambda x: 1 if x == 'M' else (-1 if x == 'F' else 0))
```

**Example with apply:**

```python
# Apply to rows (axis=1)
df.apply(lambda row: row['col1'] + row['col2'], axis=1)
```

---

## Data Aggregation

### GroupBy Operations

**Group and aggregate:**

```python
df.groupby('customer_no')[list(df.columns)[1:]].agg(lambda x: sum(x))
```

**Calculate median with conditions:**

```python
df[(df.gender == 1) & (df.alco == 0)]['bmi'].median()
```

### Pivot Tables

**Create pivot table:**

Reference: [How to Pivot a DataFrame](https://stackoverflow.com/questions/47152691/how-to-pivot-a-dataframe)

```python
# Basic pivot table
df.pivot_table(values='value', index='row', columns='col', aggfunc='mean')
```

---

## Data Cleaning

### Handling Missing Values

**Check for null values:**

```python
df.isnull().sum()  # Count nulls per column
data[pd.isnull(data).any(axis=1)]  # Rows with any null values
```

### Encoding Categorical Data

**One-Hot Encoding:**

```python
pd.get_dummies(df['ContractTime'])  # Create dummy variables
```

**Factorize (Label Encoding):**

```python
pandas.factorize(df['gender'])  # Encode as enumerated type
```

**Get unique values:**

```python
(df['col'].unique()).tolist()  # Get unique values as list
```

### Data Reshaping

**Melt (Long Format):**

```python
pd.melt(frame=df, value_vars=['gender', 'cholesterol', 'gluc'])
# Converts wide format to long format
```

---

## Data Reshaping

### Concatenation

**Concatenate DataFrames:**

```python
pd.concat([df_1, df_2], axis=1)  # axis=1: side by side
pd.concat([df_1, df_2], axis=0)  # axis=0: stacked vertically
```

### Chunking Large DataFrames

**Process DataFrame in chunks:**

```python
S = 100  # Chunk size
N = int(len(df) / S)
frames = [df.iloc[i*S:(i+1)*S].copy() for i in range(N+1)]
```

**Use Case:** Processing very large DataFrames that don't fit in memory.

---

## Export and Import

### Exporting Data

**Save Series to CSV:**

```python
pd.Series(list, dtype=int).to_csv('kill.txt', index=False)
```

**Save DataFrame:**

```python
df.to_csv('filename.csv', index=False)
df.to_excel('filename.xlsx', index=False)
df.to_json('filename.json')
```

### Importing Data

```python
df = pd.read_csv('filename.csv')
df = pd.read_excel('filename.xlsx')
df = pd.read_json('filename.json')
```

---

## Visualization

### Basic Plots

**Histogram:**

```python
data['col_5'].hist(bins=100)
```

**Scatter plot:**

```python
plt.scatter(data['col_5'], data['is_fire'])
```

**Line plot:**

```python
plt.plot(sorted(x), range(0, len(x)))
```

### Advanced Visualization

**Pair plot (Seaborn):**

```python
sns.pairplot(data[['col_12', 'is_fire']], hue='is_fire')
```

**Count plot (commented example):**

```python
# sns.countplot(x='col_97', hue='is_fire', data=data[0:10000])
```

---

## Common Patterns and Examples

### Conditional Selection and Assignment

**Select and assign:**

```python
df.id[(df['msg_qty'] > 1) & (df['std'] == 0)]
```

**Filter and sort:**

```python
df[(df['std'] > 1000) & (df['msg_qty'] == 10)].sort_values(['msg_qty'])
```

### Data Type Operations

**Check data types:**

```python
data.dtypes  # View all column types
```

**Select by data type:**

```python
obj = data.select_dtypes(include=['object'])  # Select object columns
numeric = data.select_dtypes(include=['int64', 'float64'])  # Select numeric
```

---

## Performance Tips

1. **Use vectorized operations:** Avoid loops when possible
2. **Use `.loc` and `.iloc` appropriately:** `.loc` for labels, `.iloc` for positions
3. **Avoid chained assignments:** Use `.copy()` when needed
4. **Use appropriate dtypes:** Convert to more memory-efficient types when possible
5. **Process in chunks:** For very large datasets

---

## Summary

This guide covered essential Pandas operations:

- **Inspection**: `info()`, `dtypes`, `isnull()`, `nunique()`
- **Selection**: Column selection, row filtering, conditional selection
- **Transformation**: Type conversion, string operations, function application
- **Aggregation**: GroupBy, pivot tables, statistical operations
- **Cleaning**: Handling missing values, encoding categorical data
- **Reshaping**: Melt, concatenation, chunking
- **Export/Import**: CSV, Excel, JSON operations
- **Visualization**: Histograms, scatter plots, pair plots

**Key Takeaways:**
- Use `.copy()` to avoid unintended modifications
- Prefer vectorized operations over loops
- Understand the difference between `.loc` and `.iloc`
- Use appropriate data types for memory efficiency
- Process large DataFrames in chunks when needed

Mastering these operations enables efficient data manipulation and analysis with Pandas.
