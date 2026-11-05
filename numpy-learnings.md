# NumPy Learnings

## Resources
https://numpy.org/devdocs/user/quickstart.html

## More Learning Materials
**Start here**
- [NumPy: the absolute basics for beginners](https://numpy.org/doc/stable/user/absolute_beginners.html): the official beginner guide, slower than the quickstart
- [W3Schools NumPy tutorial](https://www.w3schools.com/python/numpy/default.asp): short pages with "Try it yourself" boxes in the browser

**Go deeper**
- [Python Data Science Handbook, Ch. 2: NumPy](https://jakevdp.github.io/PythonDataScienceHandbook/02.00-introduction-to-numpy.html): free online book that covers masks and aggregations well, and leads into pandas
- [Stanford CS231n Python + NumPy tutorial](https://cs231n.github.io/python-numpy-tutorial/): one-page Python and NumPy crash course, good for someone who already codes
- [Real Python: NumPy tutorial](https://realpython.com/numpy-tutorial/): longer walkthrough with small projects, explains 2D arrays and `axis` clearly
- [Scientific Python Lectures: NumPy](https://lectures.scientific-python.org/intro/numpy/index.html): free lecture notes with exercises, good second pass after the basics
- [Broadcasting (official guide)](https://numpy.org/doc/stable/user/basics.broadcasting.html): how NumPy does math on arrays of different shapes, read once `arr * 2` feels normal

**Practice**
- [NumPy tutorials](https://numpy.org/numpy-tutorials/): official hands-on notebooks with real data
- [100 NumPy exercises](https://github.com/rougier/numpy-100): short puzzles ranked from easy to hard, with answers, like LeetCode for NumPy
- [NumPy learning hub](https://numpy.org/learn/): the official list of more courses, books, and videos

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

## Counting and Percentages
`True` counts as 1, so `sum()` counts how many items pass a test.
```python
ages = np.array([3, 7, 12, 5, 15])
(ages < 6).sum()                   # 2   -> how many children are under 6
(ages < 6).mean()                  # 0.4 -> the share under 6
round((ages < 6).mean() * 100, 1)  # 40.0 -> as a percent
```

## Median and Percentiles (Useful for "Age First Told")
The median is the middle value. One very old or very young age can't pull it around the way it pulls the mean.
```python
age_told = np.array([2, 3, 3, 4, 5, 12])
age_told.mean()                    # 4.83 -> pulled up by the 12
np.median(age_told)                # 3.5  -> the middle
np.percentile(age_told, [25, 75])  # [3. 4.75] -> the middle half of children falls in this range
```

## Counting Each Value (Like a `HashMap<Integer, Integer>`)
```python
states = np.array([6, 36, 6, 48, 6, 36])
values, counts = np.unique(states, return_counts=True)
values                             # [ 6 36 48]
counts                             # [3 2 1]  -> state 6 appears 3 times
```
In Java this is `Collectors.groupingBy(x -> x, Collectors.counting())`. In pandas you'll use `value_counts()` for the same job.

## 2D Arrays and `axis`
A 2D array is like a Java `int[][]`, but every row must be the same length.
```python
grid = np.array([[1, 2, 3],
                 [4, 5, 6]])
grid.shape                         # (2, 3) -> 2 rows, 3 columns
grid[1, 2]                         # 6      -> row 1, column 2 (Java: grid[1][2])
grid.sum(axis=0)                   # [5 7 9] -> add down each column
grid.sum(axis=1)                   # [ 6 15] -> add across each row
```
Tip: `axis=0` squashes the rows together, `axis=1` squashes the columns together. pandas uses the same idea.
