# NumPy Learnings

## Resources
https://numpy.org/devdocs/user/quickstart.html

## Java Comparison
- A NumPy array (`np.array`) is like a Java `double[]`: fixed size, one type, fast.
- Math works on the whole array at once. `arr * 2` doubles every item, with no `for` loop needed.
- `np.average(values, weights=w)` is like looping over two arrays in Java, summing `value * weight`, then dividing by the sum of the weights.

## Basics
```python
import numpy as np                 # "np" is the usual short name

arr = np.array([1, 2, 3, 4])
arr.shape                          # (4,)  -> size, like arr.length in Java
arr.dtype                          # int64 -> the one type every item has
```

## Indexing and Slicing
```python
arr[0]                             # 1     -> first item
arr[-1]                            # 4     -> last item (no Java equivalent)
arr[1:3]                           # [2 3] -> from index 1 up to, not including, 3
```

## Math on the Whole Array
```python
arr * 2                            # [2 4 6 8]
arr + arr                          # [2 4 6 8]
arr.sum(), arr.mean()              # 10, 2.5
arr.min(), arr.max()               # 1, 4
```

## Filtering (Boolean Masks)
```python
arr > 2                            # [False False True True]
arr[arr > 2]                       # [3 4] -> keep only items where the test is True
np.where(arr > 2, "big", "small")  # ['small' 'small' 'big' 'big'] -> like a ternary on every item
```
In Java this is `stream().filter(x -> x > 2)`.

## Missing Values (NaN)
`np.nan` means "no value", like `null` for a number. Normal math breaks on it.
```python
x = np.array([1.0, np.nan, 3.0])
x.mean()                           # nan -> one missing value spoils the result
np.nanmean(x)                      # 2.0 -> skips the missing value
np.isnan(x)                        # [False True False]
```

## Weighted Average (Used in This Project)
`True` counts as 1 and `False` as 0, so the average of yes/no answers is a rate.
```python
has_autism = np.array([True, False, False, True])
weights    = np.array([100, 300, 200, 100])  # how many real children each answer stands for

has_autism.mean()                            # 0.5   -> plain rate: 2 of 4 answers
np.average(has_autism, weights=weights)      # 0.286 -> (100 + 100) / 700
```
The plain and weighted rates can differ a lot. That's why the project uses the survey weight `fwc`.
