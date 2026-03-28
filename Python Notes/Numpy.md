Here's the **enhanced and more detailed NumPy Notes** with:

- Clear description of **each major method/function**
- Proper code example
- Exact expected output (verified)
- Usage explanation (when and why to use it)

---

# Comprehensive NumPy Notes with Descriptions, Examples & Outputs

**Import Convention** (Use this in every script/notebook):
```python
import numpy as np
```

---

## 1. Introduction to NumPy

**NumPy (Numerical Python)** is the core library for numerical computing in Python. It introduces the `ndarray` (n-dimensional array) object which is fast, memory-efficient, and supports vectorized operations.

**Key Benefits**:
- Much faster than Python lists for numerical tasks (due to contiguous memory and C implementation).
- Supports **Broadcasting** and **Universal Functions (ufuncs)**.
- Foundation for Pandas, SciPy, scikit-learn, TensorFlow, etc.

---

## 2. Creating NumPy Arrays

### `np.array(object, dtype=None)`
**Description**: Creates a NumPy array from Python lists, tuples, or other sequences. Most basic and commonly used method.

**Example**:
```python
# 1D array
arr1 = np.array([1, 2, 3, 4, 5])
print("1D Array:")
print(arr1)

# 2D array
arr2 = np.array([[1, 2, 3], [4, 5, 6]])
print("\n2D Array:")
print(arr2)
```

**Output**:
```
1D Array:
[1 2 3 4 5]

2D Array:
[[1 2 3]
 [4 5 6]]
```

**Usage**: When you already have data in Python list format.

### `np.zeros(shape, dtype=float)`
**Description**: Returns a new array of given shape filled with zeros. Useful for initialization.

**Example**:
```python
print(np.zeros((2, 3)))
```

**Output**:
```
[[0. 0. 0.]
 [0. 0. 0.]]
```

### `np.ones(shape, dtype=float)`
**Description**: Returns a new array of given shape filled with ones.

**Example**:
```python
print(np.ones((3, 2)))
```

**Output**:
```
[[1. 1.]
 [1. 1.]
 [1. 1.]]
```

### `np.full(shape, fill_value)`
**Description**: Creates an array filled with a specific value.

**Example**:
```python
print(np.full((2, 2), 7))
```

**Output**:
```
[[7 7]
 [7 7]]
```

### `np.eye(N, M=None, k=0)`
**Description**: Creates a 2D identity matrix (ones on diagonal, zeros elsewhere).

**Example**:
```python
print(np.eye(3))
```

**Output**:
```
[[1. 0. 0.]
 [0. 1. 0.]
 [0. 0. 1.]]
```

### `np.arange(start, stop, step=1)`
**Description**: Returns evenly spaced values within a given interval **[start, stop)** with given step.

**Example**:
```python
print(np.arange(0, 10, 2))
```

**Output**:
```
[0 2 4 6 8]
```

**Usage**: When you need a sequence of numbers with fixed step (like range() but returns array).

### `np.linspace(start, stop, num=50)`
**Description**: Returns `num` evenly spaced samples over the interval [start, stop] (inclusive of both ends).

**Example**:
```python
print(np.linspace(0, 1, 5))
```

**Output**:
```
[0.   0.25 0.5  0.75 1.  ]
```

**Usage**: Perfect for plotting, creating smooth ranges, or generating test data.

### `np.random.random(shape)` / `np.random.rand()`
**Description**: Generates random floats in the half-open interval [0.0, 1.0).

**Example**:
```python
np.random.seed(0)   # for reproducibility
print(np.random.random((2, 2)))
```

**Output**:
```
[[0.5488135  0.71518937]
 [0.60276338 0.54488318]]
```

---

## 3. Array Attributes

```python
a = np.array([[1, 2, 3], [4, 5, 6]], dtype=np.int32)

print("ndim     :", a.ndim)      # Number of dimensions
print("shape    :", a.shape)     # Tuple of dimensions
print("size     :", a.size)      # Total number of elements
print("dtype    :", a.dtype)     # Data type of elements
print("itemsize :", a.itemsize)  # Bytes per element
print("nbytes   :", a.nbytes)    # Total bytes consumed
```

**Typical Output**:
```
ndim     : 2
shape    : (2, 3)
size     : 6
dtype    : int32
itemsize : 4
nbytes   : 24
```

**Usage**: Very important for debugging shape mismatches in ML/DL.

---

## 4. Indexing and Slicing

### Basic Indexing & Slicing
**Description**: Access elements using indices. Slicing uses `start:stop:step`.

**Example**:
```python
arr2d = np.array([[1,2,3],[4,5,6]])
print("Original array:")
print(arr2d)

print("\nElement at [0,1]:", arr2d[0,1])
print("First row:", arr2d[0, :])
print("First column:", arr2d[:, 0])
print("Sub-matrix:")
print(arr2d[0:2, 1:3])
```

**Output**:
```
Original array:
[[1 2 3]
 [4 5 6]]

Element at [0,1]: 2
First row: [1 2 3]
First column: [1 4]
Sub-matrix:
[[2 3]
 [5 6]]
```

**Note**: Slicing creates a **view** (not a copy). Changes in slice affect the original array.

### Boolean Indexing (Masking)
**Description**: Use boolean conditions to filter elements.

**Example**:
```python
arr = np.array([10, 20, 30, 40, 50])
print(arr[arr > 25])        # elements greater than 25
```

**Output**:
```
[30 40 50]
```

---

## 5. Array Manipulation

### `reshape(new_shape)`
**Description**: Returns a new view with changed shape (without changing data).

**Example**:
```python
a = np.arange(12)
print("Original:", a)
print("Reshaped to 3x4:")
print(a.reshape(3, 4))
```

### `flatten()` vs `ravel()`
- `flatten()`: Always returns a **copy**.
- `ravel()`: Returns a **view** when possible (more memory efficient).

### Stacking Functions
- `np.vstack()` → Stack arrays vertically (row-wise)
- `np.hstack()` → Stack arrays horizontally (column-wise)

**Example**:
```python
a = np.array([[1,2],[3,4]])
b = np.array([[5,6],[7,8]])
print("Vertical Stack:")
print(np.vstack((a, b)))
```

---

## 6. Broadcasting

**Description**: NumPy automatically expands smaller arrays to match the shape of larger ones during arithmetic operations.

**Example**:
```python
a = np.array([1, 2, 3])           # shape (3,)
c = np.array([[1],[2],[3]])       # shape (3,1)

print("a shape:", a.shape)
print("c shape:", c.shape)
print("a + c result:")
print(a + c)
```

**Output**:
```
a shape: (3,)
c shape: (3, 1)
a + c result:
[[2 3 4]
 [3 4 5]
 [4 5 6]]
```

**Usage**: Extremely powerful — avoids writing loops for operations like adding a bias or scaling features.

---

## 7. Mathematical & Statistical Functions

### Aggregation Functions
**Description**: `sum()`, `mean()`, `std()`, `min()`, `max()`, etc. Support `axis` parameter.

**Example**:
```python
arr = np.array([[1,2,3],[4,5,6]])

print("Sum of all elements:", np.sum(arr))
print("Sum along columns (axis=0):", np.sum(arr, axis=0))
print("Sum along rows (axis=1):", np.sum(arr, axis=1))
print("Mean:", np.mean(arr))
```

**Output**:
```
Sum of all elements: 21
Sum along columns (axis=0): [5 7 9]
Sum along rows (axis=1): [ 6 15]
Mean: 3.5
```

**Usage**: Calculating statistics across features (columns) or samples (rows) in datasets.

---

## 8. Linear Algebra

### `np.dot()` or `@` operator (Matrix Multiplication)
**Example**:
```python
A = np.array([[1,2],[3,4]])
B = np.array([[5,6],[7,8]])
print("A @ B:")
print(A @ B)
```

**Output**:
```
A @ B:
[[19 22]
 [43 50]]
```

### Other Important Functions
- `np.linalg.inv(A)` → Matrix inverse
- `np.linalg.det(A)` → Determinant
- `np.linalg.eig(A)` → Eigenvalues and eigenvectors
- `np.linalg.solve(A, b)` → Solve linear system Ax = b

---

## 9. Random Module (`np.random`)

**Important Methods**:
- `np.random.seed(seed)` → Set seed for reproducibility
- `np.random.rand()` → Uniform [0, 1)
- `np.random.randn()` → Standard normal distribution (mean=0, std=1)
- `np.random.randint(low, high, size)` → Random integers

**Usage in ML**: Weight initialization, data augmentation, Monte Carlo simulations.

---

**Practice Tip**:  
Run all these examples in a Jupyter notebook. Try modifying shapes and see how broadcasting and reshaping behave.

Would you like me to add the next sections with the same level of detail (Universal Functions, Sorting, File I/O, Performance Tips, and Common Use Cases with full outputs)? Just say **"continue"** or tell me which specific topic you want expanded next.
