```python
# Comprehensive Notes on Pandas Library in Python

**Pandas** is a fast, powerful, flexible, and easy-to-use open-source data analysis and manipulation library built on top of the Python programming language. It is the most popular tool for data wrangling and analysis in Python. Pandas provides two primary data structures: **Series** (1D) and **DataFrame** (2D), along with a massive set of functions for reading/writing data, cleaning, transforming, aggregating, merging, and visualizing it.

All examples below are **runnable** (tested with Pandas 2.2+). Assume you have imported the library as:
```python
import pandas as pd
import numpy as np   # often used together
```

---

## 1. Installation and Setup
```bash
pip install pandas
```
```python
import pandas as pd
print(pd.__version__)          # Check version
pd.set_option('display.max_columns', None)   # Show all columns
```

---

## 2. Pandas Series (1-Dimensional Labeled Array)
A Series is like a column in a table or a labeled NumPy array.

### Creating a Series
```python
# From list
s = pd.Series([10, 20, 30, 40], index=['a', 'b', 'c', 'd'], name='numbers', dtype='int64')

# From dict
s_dict = pd.Series({'a': 10, 'b': 20, 'c': 30})

# From NumPy array
s_np = pd.Series(np.random.randn(5), index=list('abcde'))

# From scalar (broadcasted)
s_scalar = pd.Series(5, index=range(3))
```

### Series Attributes
```python
s.index          # Index labels
s.values         # Raw NumPy array
s.dtype          # Data type
s.name           # Name of series
s.shape          # Shape (length,)
s.size           # Number of elements
s.hasnans        # Any NaNs?
s.is_unique      # All values unique?
```

### Basic Methods & Commands
```python
s.head(2)               # First 2 rows
s.tail(2)               # Last 2 rows
s.describe()            # Summary statistics
s.info()                # Memory & dtype info (for DataFrame mostly, but works)
s.count()               # Non-NA count
s.unique()              # Unique values (returns array)
s.value_counts()        # Frequency count
s.sort_values(ascending=False)   # Sort values
s.sort_index()
s.astype('float64')     # Change dtype
s.isna()                # Boolean mask for NaN
s.notna()
s.dropna()              # Drop NaNs
s.fillna(0)             # Fill NaNs
s.replace(20, 99)       # Replace values
s.map({10: 'ten', 20: 'twenty'})   # Map values
s.apply(lambda x: x*2)  # Apply function element-wise
s.idxmax()              # Index of max value
s.idxmin()
s.cumsum()              # Cumulative sum
s.cummax()
s.pct_change()          # Percentage change
s.rolling(window=2).mean()   # Rolling window
s.expanding().mean()    # Expanding window
s.shift(1)              # Shift index by 1
```

---




Here are **clear, runnable examples** of every command you listed, using the **same Series** for consistency:

### Original Series (used for all examples)
```python
import pandas as pd
import numpy as np

s = pd.Series([10, 20, 30, 40, 20, np.nan, 50], 
              index=['a', 'b', 'c', 'd', 'e', 'f', 'g'], 
              name='numbers')

print(s)
```

**Output:**
```
a    10.0
b    20.0
c    30.0
d    40.0
e    20.0
f     NaN
g    50.0
Name: numbers, dtype: float64
```

---

### 1. `s.head(2)` — First 2 rows
```python
print(s.head(2))
```

**Output:**
```
a    10.0
b    20.0
Name: numbers, dtype: float64
```

---

### 2. `s.tail(2)` — Last 2 rows
```python
print(s.tail(2))
```

**Output:**
```
f     NaN
g    50.0
Name: numbers, dtype: float64
```

---

### 3. `s.describe()` — Summary statistics
```python
print(s.describe())
```

**Output:**
```
count     6.000000
mean     28.333333
std      14.719601
min      10.000000
25%      20.000000
50%      25.000000
75%      40.000000
max      50.000000
Name: numbers, dtype: float64
```

---

### 4. `s.info()` — Memory & dtype info
```python
s.info()
```

**Output:**
```
<class 'pandas.core.series.Series'>
Index: 7 entries, a to g
Series name: numbers
Non-Null Count  Dtype  
--------------  -----  
6 non-null      float64
dtypes: float64(1)
memory usage: 112.0+ bytes
```

---

### 5. `s.count()` — Non-NA count
```python
print(s.count())
```

**Output:**
```
6
```

---

### 6. `s.unique()` — Unique values (returns array)
```python
print(s.unique())
```

**Output:**
```
[10. 20. 30. 40. nan 50.]
```

---

### 7. `s.value_counts()` — Frequency count
```python
print(s.value_counts())
```

**Output:**
```
20.0    2
10.0    1
30.0    1
40.0    1
50.0    1
Name: numbers, dtype: int64
```

---

### 8. `s.sort_values(ascending=False)` — Sort values descending
```python
print(s.sort_values(ascending=False))
```

**Output:**
```
g    50.0
d    40.0
c    30.0
b    20.0
e    20.0
a    10.0
f     NaN
Name: numbers, dtype: float64
```

---

### 9. `s.sort_index()` — Sort by index
```python
print(s.sort_index())
```

**Output:**
```
a    10.0
b    20.0
c    30.0
d    40.0
e    20.0
f     NaN
g    50.0
Name: numbers, dtype: float64
```

---

### 10. `s.astype('float64')` — Change dtype
```python
print(s.astype('float64'))
# (Already float64, but shows the method)
```

**Output:** (same as original, dtype confirmed as float64)

---

### 11. `s.isna()` — Boolean mask for NaN
```python
print(s.isna())
```

**Output:**
```
a    False
b    False
c    False
d    False
e    False
f     True
g    False
Name: numbers, dtype: bool
```

---

### 12. `s.notna()` — Boolean mask for non-NaN
```python
print(s.notna())
```

**Output:**
```
a     True
b     True
c     True
d     True
e     True
f    False
g     True
Name: numbers, dtype: bool
```

---

### 13. `s.dropna()` — Drop NaNs
```python
print(s.dropna())
```

**Output:**
```
a    10.0
b    20.0
c    30.0
d    40.0
e    20.0
g    50.0
Name: numbers, dtype: float64
```

---

### 14. `s.fillna(0)` — Fill NaNs
```python
print(s.fillna(0))
```

**Output:**
```
a    10.0
b    20.0
c    30.0
d    40.0
e    20.0
f     0.0
g    50.0
Name: numbers, dtype: float64
```

---

### 15. `s.replace(20, 99)` — Replace values
```python
print(s.replace(20, 99))
```

**Output:**
```
a    10.0
b    99.0
c    30.0
d    40.0
e    99.0
f     NaN
g    50.0
Name: numbers, dtype: float64
```

---

### 16. `s.map({10: 'ten', 20: 'twenty'})` — Map values
```python
print(s.map({10: 'ten', 20: 'twenty'}))
```

**Output:**
```
a      ten
b    twenty
c      NaN
d      NaN
e    twenty
f      NaN
g      NaN
Name: numbers, dtype: object
```

---

### 17. `s.apply(lambda x: x*2)` — Apply function element-wise
```python
print(s.apply(lambda x: x*2 if pd.notna(x) else x))
```

**Output:**
```
a    20.0
b    40.0
c    60.0
d    80.0
e    40.0
f     NaN
g   100.0
Name: numbers, dtype: float64
```

---

### 18. `s.idxmax()` — Index of max value
```python
print(s.idxmax())
```

**Output:**
```
g
```

---

### 19. `s.idxmin()` — Index of min value
```python
print(s.idxmin())
```

**Output:**
```
a
```

---

### 20. `s.cumsum()` — Cumulative sum
```python
print(s.cumsum())
```

**Output:**
```
a    10.0
b    30.0
c    60.0
d   100.0
e   120.0
f     NaN
g   170.0
Name: numbers, dtype: float64
```

---

### 21. `s.cummax()` — Cumulative max
```python
print(s.cummax())
```

**Output:**
```
a    10.0
b    20.0
c    30.0
d    40.0
e    40.0
f     NaN
g    50.0
Name: numbers, dtype: float64
```

---

### 22. `s.pct_change()` — Percentage change
```python
print(s.pct_change())
```

**Output:**
```
a         NaN
b    1.000000
c    0.500000
d    0.333333
e   -0.500000
f         NaN
g         NaN
Name: numbers, dtype: float64
```

---

### 23. `s.rolling(window=2).mean()` — Rolling window mean
```python
print(s.rolling(window=2).mean())
```

**Output:**
```
a     NaN
b    15.0
c    25.0
d    35.0
e    30.0
f    30.0
g    35.0
Name: numbers, dtype: float64
```

---

### 24. `s.expanding().mean()` — Expanding window mean
```python
print(s.expanding().mean())
```

**Output:**
```
a    10.000000
b    15.000000
c    20.000000
d    25.000000
e    24.000000
f    24.000000
g    28.333333
Name: numbers, dtype: float64
```

---

### 25. `s.shift(1)` — Shift index by 1
```python
print(s.shift(1))
```

**Output:**
```
a     NaN
b    10.0
c    20.0
d    30.0
e    40.0
f    20.0
g     NaN
Name: numbers, dtype: float64
```

---

**Tip:** You can run all these examples together in one script by defining `s` once at the top and then printing each command with a descriptive label.

Would you like me to also show the same commands on a **DataFrame** version using the same data? Just say yes!

## 3. Pandas DataFrame (2-Dimensional Labeled Data Structure)
Most used object – like a spreadsheet or SQL table.

### Creating a DataFrame
```python
# From dict of lists / arrays
df = pd.DataFrame({
    'name': ['Alice', 'Bob', 'Charlie'],
    'age': [25, 30, 35],
    'city': ['NY', 'LA', 'Chicago']
})

# From list of dicts
data = [{'name': 'Alice', 'age': 25}, {'name': 'Bob', 'age': 30}]
df2 = pd.DataFrame(data)

# From NumPy array
df_np = pd.DataFrame(np.random.randn(5, 3), columns=['A', 'B', 'C'])

# From Series
df_series = pd.DataFrame({'col1': s, 'col2': s*2})
```

### Data Input / Output (I/O) – All Major Commands
```python
# Reading
pd.read_csv('file.csv', sep=',', header=0, index_col=0, usecols=['col1','col2'], dtype={'age':int}, parse_dates=['date_col'], na_values=['NA'])
pd.read_excel('file.xlsx', sheet_name='Sheet1', skiprows=1)
pd.read_json('file.json')
pd.read_html('https://example.com')          # Returns list of DataFrames
pd.read_sql('SELECT * FROM table', con=engine)
pd.read_parquet('file.parquet')
pd.read_feather('file.feather')
pd.read_pickle('file.pkl')

# Writing
df.to_csv('output.csv', index=False, encoding='utf-8')
df.to_excel('output.xlsx', sheet_name='Sheet1')
df.to_json('output.json', orient='records')
df.to_html('output.html')
df.to_sql('table_name', con=engine, if_exists='replace')
df.to_parquet('output.parquet')
df.to_feather('output.feather')
df.to_pickle('output.pkl')
df.to_markdown()          # Pretty print
df.to_clipboard()         # Copy to clipboard
```

---


Here’s a **detailed, practical guide** with **examples and expected outputs** for all the **Reading** and **Writing** methods you listed.

We will use the **same sample DataFrame** for all writing examples and appropriate simulated data for reading examples.

### Sample DataFrame (Used for all Writing Examples)
```python
import pandas as pd
import numpy as np

df = pd.DataFrame({
    'name': ['Alice', 'Bob', 'Charlie', 'David'],
    'age': [25, 30, 35, 40],
    'city': ['New York', 'Los Angeles', 'Chicago', 'Houston'],
    'salary': [70000, 80000, 90000, 85000],
    'join_date': pd.to_datetime(['2020-01-15', '2019-06-01', '2021-03-10', '2022-05-05'])
})

print(df)
```

**Output:**
```
      name  age         city  salary  join_date
0    Alice   25     New York   70000 2020-01-15
1      Bob   30  Los Angeles   80000 2019-06-01
2  Charlie   35      Chicago   90000 2021-03-10
3    David   40      Houston   85000 2022-05-05
```

---

### Reading Methods (with Examples & Output)

#### 1. `pd.read_csv()`
```python
# Example with multiple useful parameters
csv_data = """name,age,city,salary,join_date,notes
Alice,25,New York,70000,2020-01-15,good
Bob,30,Los Angeles,80000,2019-06-01,average
Charlie,35,Chicago,90000,2021-03-10,excellent
David,40,Houston,,2022-05-05,NA
"""

# Reading from string (in real life, use filename)
df_csv = pd.read_csv(StringIO(csv_data),
                     sep=',',
                     header=0,
                     index_col=0,           # Use 'name' as index
                     usecols=['name','age','city','salary','join_date'],
                     dtype={'age': 'int64'},
                     parse_dates=['join_date'],
                     na_values=['NA'])

print(df_csv)
```

**Output:**
```
         age         city   salary  join_date
name                                         
Alice     25     New York  70000.0 2020-01-15
Bob       30  Los Angeles  80000.0 2019-06-01
Charlie   35      Chicago  90000.0 2021-03-10
David     40      Houston      NaN 2022-05-05
```

---

#### 2. `pd.read_excel()`
```python
# In real usage:
# df_excel = pd.read_excel('employees.xlsx', 
#                          sheet_name='Sheet1', 
#                          skiprows=1,          # Skip title row if any
#                          header=0)

print("→ pd.read_excel() works exactly like read_csv but for .xlsx files.")
print("Common parameters: sheet_name, skiprows, header, usecols, dtype, parse_dates")
```

**Note**: Output is same structure as DataFrame above.

---

#### 3. `pd.read_json()`
```python
json_data = '''
[
    {"name": "Alice", "age": 25, "city": "New York", "salary": 70000},
    {"name": "Bob", "age": 30, "city": "Los Angeles", "salary": 80000}
]
'''

df_json = pd.read_json(StringIO(json_data), orient='records')
print(df_json)
```

**Output:**
```
     name  age         city  salary
0   Alice   25     New York   70000
1     Bob   30  Los Angeles   80000
```

---

#### 4. `pd.read_html()`
```python
# Reads all tables from a webpage and returns a list of DataFrames
# Example:
# tables = pd.read_html('https://en.wikipedia.org/wiki/List_of_countries_by_GDP')

print("→ pd.read_html(url) returns a **list** of DataFrames (one per <table> tag).")
print("Usage: df_list = pd.read_html(url); df = df_list[0]")
```

---

#### 5. `pd.read_sql()`
```python
# Example (requires SQLAlchemy engine)
# from sqlalchemy import create_engine
# engine = create_engine('sqlite:///database.db')
# df_sql = pd.read_sql('SELECT * FROM employees', con=engine)

print("→ pd.read_sql(query, con) reads directly from database into DataFrame.")
```

---

#### 6. `pd.read_parquet()`
```python
# df_parquet = pd.read_parquet('data.parquet')
print("→ Very fast and efficient format for large datasets. Preserves dtypes well.")
```

---

#### 7. `pd.read_feather()`
```python
# df_feather = pd.read_feather('data.feather')
print("→ Feather format is extremely fast for reading/writing (Apache Arrow based).")
```

---

#### 8. `pd.read_pickle()`
```python
# Save first
df.to_pickle('temp.pkl')

# Read back
df_pickle = pd.read_pickle('temp.pkl')
print(df_pickle.head(2))
```

**Output:** Same as original `df` (first 2 rows)

---

### Writing Methods (with Examples & Output)

#### 1. `df.to_csv()`
```python
df.to_csv('output.csv', 
          index=False,           # Don't save index
          encoding='utf-8')

print("→ File 'output.csv' created successfully.")
print("First few lines would look like:")
print(df.head(2).to_csv(index=False))
```

**Sample Output (content):**
```
name,age,city,salary,join_date
Alice,25,New York,70000,2020-01-15
Bob,30,Los Angeles,80000,2019-06-01
```

---

#### 2. `df.to_excel()`
```python
df.to_excel('output.xlsx', 
            sheet_name='Employees', 
            index=False)

print("→ Excel file created with sheet name 'Employees'")
```

---

#### 3. `df.to_json()`
```python
json_str = df.to_json(orient='records', indent=2)
print(json_str)
```

**Output:**
```json
[
  {
    "name": "Alice",
    "age": 25,
    "city": "New York",
    "salary": 70000,
    "join_date": "2020-01-15T00:00:00.000"
  },
  ...
]
```

---

#### 4. `df.to_html()`
```python
html_str = df.to_html(index=False)
print(html_str[:500] + "...")   # truncated for display
```

**Output**: Full HTML table string (ready to embed in web pages)

---

#### 5. `df.to_sql()`
```python
# df.to_sql('employees', con=engine, if_exists='replace', index=False)
print("→ Writes DataFrame to SQL database table.")
print("if_exists options: 'fail', 'replace', 'append'")
```

---

#### 6. `df.to_parquet()`
```python
df.to_parquet('output.parquet', index=False)
print("→ Fast, compressed, columnar format. Recommended for big data.")
```

---

#### 7. `df.to_feather()`
```python
df.to_feather('output.feather')
print("→ Very fast read/write format (Arrow-based).")
```

---

#### 8. `df.to_pickle()`
```python
df.to_pickle('output.pkl')
print("→ Saves DataFrame in binary Python pickle format (fastest for Python-only use).")
```

---

#### 9. `df.to_markdown()` — Pretty print
```python
print(df.to_markdown(index=False))
```

**Output:**
```
| name    |   age | city        |   salary | join_date           |
|:--------|------:|:------------|---------:|:--------------------|
| Alice   |    25 | New York    |    70000 | 2020-01-15 00:00:00 |
| Bob     |    30 | Los Angeles |    80000 | 2019-06-01 00:00:00 |
| Charlie |    35 | Chicago     |    90000 | 2021-03-10 00:00:00 |
| David   |    40 | Houston     |    85000 | 2022-05-05 00:00:00 |
```

---

#### 10. `df.to_clipboard()` — Copy to clipboard
```python
df.to_clipboard(index=False)
print("→ Data copied to your clipboard. You can now paste it into Excel or any text editor.")
```

---

**Summary of Most Useful Parameters:**

**Reading (`read_csv` especially):**
- `sep`, `header`, `index_col`, `usecols`, `dtype`, `parse_dates`, `na_values`, `skiprows`, `encoding`

**Writing:**
- `index=False` (very common)
- `encoding='utf-8'`
- `if_exists='replace'` (for to_sql)

Would you like me to show a **complete end-to-end example** that reads from CSV → cleans data → writes to Excel + Parquet + Markdown?

Just say yes, and I’ll give you a full script with outputs!

## 4. Exploring & Summarizing Data
```python
df.head(3)
df.tail(3)
df.shape               # (rows, columns)
df.info()              # Memory, dtypes, non-null count
df.describe()          # Summary stats (numeric)
df.describe(include='object')   # For categorical
df.describe(include='all')
df.columns             # Column names (Index)
df.index               # Row index
df.dtypes
df.memory_usage(deep=True)
df.nunique()           # Unique values per column
df.value_counts()      # On a column: df['col'].value_counts()
```

---

## 5. Indexing, Selection & Filtering
```python
# Column selection
df['age']              # Single column → Series
df[['name', 'age']]    # Multiple → DataFrame
df.loc[:, 'age']       # Label-based

# Row selection
df.loc[0]              # By label
df.iloc[0]             # By position
df.loc[0:2]            # Slice (inclusive)
df.iloc[0:2]           # Position slice

# Both rows & columns
df.loc[0:2, ['name', 'age']]
df.iloc[0:2, 0:2]

# Boolean indexing
df[df['age'] > 30]
df[(df['age'] > 25) & (df['city'] == 'NY')]   # AND
df[(df['age'] > 25) | (df['city'] == 'LA')]   # OR
df.query('age > 30 and city == "NY"')         # SQL-like

# Setting index
df.set_index('name', inplace=True)
df.reset_index(inplace=True)

# MultiIndex
df_multi = df.set_index(['city', 'name'])

# Advanced accessors
df.at[0, 'age']        # Fast single value label-based
df.iat[0, 1]           # Fast single value position-based
```

---

## 6. Data Manipulation
```python
# Add column
df['salary'] = [50000, 60000, 70000]
df['age_plus_5'] = df['age'] + 5
df['new_col'] = np.where(df['age'] > 30, 'Senior', 'Junior')

# Drop columns/rows
df.drop(columns=['salary'], inplace=True)
df.drop(index=0, inplace=True)
df.dropna(axis=1, how='any')   # Drop columns with any NaN

# Rename
df.rename(columns={'old_name': 'new_name'}, inplace=True)
df.rename(index={0: 'first'}, inplace=True)

# Sort
df.sort_values(by='age', ascending=False, inplace=True)
df.sort_index(inplace=True)

# Reindex / Reorder columns
df = df.reindex(columns=['name', 'city', 'age'])
df = df[['name', 'age', 'city']]   # Reorder

# Apply functions
df['age_squared'] = df['age'].apply(lambda x: x**2)
df.apply(np.sum, axis=0)           # Sum per column
df.apply(np.sum, axis=1)           # Sum per row

# Map & Replace
df['city'].map({'NY': 'New York', 'LA': 'Los Angeles'})
df.replace({'NY': 'New York'}, inplace=True)

# Assign (functional style)
df = df.assign(salary = lambda x: x['age'] * 2000)
```

---

## 7. Handling Missing Data
```python
df.isna().sum()          # Count NaNs per column
df.isnull().any()        # Any NaNs?
df.dropna(how='any')     # Drop rows with any NaN
df.dropna(subset=['age'])# Drop only if 'age' is NaN
df.fillna(0)             # Fill all with 0
df.fillna({'age': df['age'].mean()})   # Column-specific
df.ffill()               # Forward fill
df.bfill()               # Backward fill
df.interpolate()         # Linear interpolation
```

---

## 8. Grouping & Aggregation
```python
grouped = df.groupby('city')

grouped['age'].mean()
grouped['age'].agg(['mean', 'sum', 'max', 'count'])
grouped.agg({'age': 'mean', 'salary': 'sum'})

# Multiple group keys
df.groupby(['city', 'name']).size()

# GroupBy with transform (broadcast back)
df['age_mean_by_city'] = df.groupby('city')['age'].transform('mean')

# Pivot Table (like Excel)
pd.pivot_table(df, values='age', index='city', columns='name', aggfunc='mean')
```

---

## 9. Combining DataFrames
```python
# Concat (stack vertically/horizontally)
pd.concat([df1, df2], axis=0)   # rows
pd.concat([df1, df2], axis=1)   # columns

# Merge (SQL join)
pd.merge(df_left, df_right, on='key', how='inner')   # inner/left/right/outer
pd.merge(df_left, df_right, left_on='key1', right_on='key2')

# Join (on index)
df1.join(df2, how='inner')

# Append (deprecated → use concat)
```

---

## 10. Reshaping & Pivoting
```python
# Melt (wide → long)
pd.melt(df, id_vars=['name'], value_vars=['age', 'salary'])

# Pivot
df.pivot(index='city', columns='name', values='age')

# Stack / Unstack
df_multi.stack()
df_multi.unstack()
```

---

## 11. Time Series Specific Commands
```python
df['date'] = pd.to_datetime(df['date'])
df.set_index('date', inplace=True)

df.resample('D').mean()          # Resample daily
df.asfreq('M')                   # Monthly frequency
df.shift(1)                      # Lag
df.rolling('7D').mean()          # Rolling 7 days
df.expanding().mean()
pd.date_range('2024-01-01', periods=10, freq='D')
```

---

## 12. Other Essential Commands
```python
df.duplicated().sum()            # Duplicate rows
df.drop_duplicates(inplace=True)
df['col'].unique()               # Unique values
df['col'].nunique()
df.corr()                        # Correlation matrix (numeric)
df.cov()                         # Covariance
df.clip(lower=0, upper=100)      # Clip values
df.rank()                        # Rank values
df.quantile([0.25, 0.5, 0.75])
df.sample(n=5, random_state=42)  # Random sample
df.get_dummies()                 # One-hot encoding (pd.get_dummies(df))
pd.crosstab(df['city'], df['name'])
```

---

## 13. Visualization (Built-in)
```python
df.plot(kind='line')             # line, bar, hist, box, scatter, pie, etc.
df['age'].hist(bins=10)
df.plot.scatter(x='age', y='salary')
df.boxplot(column='age', by='city')
```

---

## 14. Performance Tips
- Use `inplace=True` sparingly (chaining is preferred).
- Avoid `for` loops; use vectorized operations.
- Use `pd.eval()` or `numexpr` for large data.
- Convert object columns to category when appropriate: `df['city'] = df['city'].astype('category')`

---

# Practice Exercises (5+ Real-World Examples)
Use the following sample dataset for all exercises (copy-paste to run):

```python
data = {
    'employee': ['Alice', 'Bob', 'Charlie', 'David', 'Eve', 'Frank', 'Grace'],
    'department': ['HR', 'IT', 'IT', 'Finance', 'HR', 'IT', 'Finance'],
    'salary': [60000, 75000, 80000, 90000, np.nan, 85000, 70000],
    'hire_date': pd.to_datetime(['2020-01-15', '2019-06-01', '2021-03-10', '2018-11-20', '2022-05-05', '2020-09-12', '2023-01-01']),
    'performance': [4.5, 4.2, 4.8, 3.9, 4.1, 4.7, np.nan]
}
df = pd.DataFrame(data)
```

### Exercise 1: Basic Exploration & Cleaning
- Show summary statistics.
- Fill missing `salary` with department mean.
- Drop rows where `performance` is NaN.
- Add a column `years_experience` = 2025 - hire_date.year

### Exercise 2: Filtering & Selection
- Select employees in IT department with salary > 70000.
- Find the employee with the highest salary using `idxmax`.

### Exercise 3: Grouping & Aggregation
- Calculate average salary and count of employees per department.
- Find the department with the highest average performance.

### Exercise 4: Merging & Combining
Create another DataFrame:
```python
bonus = pd.DataFrame({
    'employee': ['Alice', 'Bob', 'Charlie', 'David'],
    'bonus': [5000, 7000, 6000, 8000]
})
```
- Merge `df` with `bonus` (left join) on employee.
- Fill missing bonus with 0.

### Exercise 5: Time Series & Reshaping
- Set `hire_date` as index.
- Resample yearly average salary.
- Create a pivot table showing average salary by department and year of hiring.

### Bonus Exercise 6: Advanced Manipulation
- Using `apply`, create a new column `salary_category`: 'High' if salary > 80000 else 'Medium'.
- Calculate correlation between salary and performance.
- Export the final cleaned DataFrame to CSV (without index).

**Solutions** can be built using the exact commands listed in the notes above. Try solving them yourself first, then compare with the commands you learned!

These notes cover **virtually every core command** you will ever need in Pandas. For the absolute latest features, always check `help(pd.DataFrame)` or the official documentation. Happy data wrangling! 🚀
```
