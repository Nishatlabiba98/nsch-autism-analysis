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

## Out-of-the-Box Analogy: A Marching Band
- **A Python list is a crowd at a mall.** Anyone can be in it, of any type. To get something done, you walk up to each person one at a time (a `for` loop).
- **A NumPy array is a marching band in formation.** Everyone wears the same uniform (`dtype`) and stands in a fixed grid (`shape`), and each spot has a number (index).
- **`arr * 2` is the conductor shouting "everyone, double your step!"** It's one command and the whole band moves at once. No loop needed.
- **A mask is "everyone holding a trumpet, step forward."** `arr[arr > 2]` keeps only the players who pass the test.
- **A 2D array is the band's rows and columns.** `sum(axis=0)` counts down each column, and `sum(axis=1)` counts across each row.
- **`NaN` is an empty spot in the formation.** Normal math trips over it, and `np.nanmean` steps around it.
- **A view is a spotlight on part of the band.** Anything you change under the spotlight changes the real players. `.copy()` hires a second band.
- **A weighted average is the tuba being louder than the flute.** Both play, but the tuba counts for more in the overall sound, the way a survey answer with a big weight counts for more children.

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

## Making Arrays Without Typing Every Value
```python
np.zeros(3)                        # [0. 0. 0.]  -> like new double[3] in Java
np.ones(3)                         # [1. 1. 1.]
np.arange(0, 10, 2)                # [0 2 4 6 8] -> like a for loop: start, stop (not included), step
np.linspace(0, 1, 5)               # [0. 0.25 0.5 0.75 1.] -> 5 evenly spaced values, end included
```

## Changing the Type (`astype`)
Like casting in Java.
```python
answers = np.array([1, 2, 2, 1])   # survey style: 1 = yes, 2 = no
answers == 1                       # [ True False False  True] -> now yes/no
(answers == 1).astype(int)         # [1 0 0 1]
np.array([1.7, 2.2]).astype(int)   # [1 2] -> cuts off the decimal, like (int) 1.7 in Java
```

## Combining Tests (`&`, `|`, `~`)
Use `&` (and), `|` (or), `~` (not), **not** Java's `&&`, `||`, `!` or Python's `and`, `or`, `not`. **Put each test in brackets.**
```python
ages = np.array([3, 7, 12, 5, 15])
ages[(ages >= 6) & (ages < 12)]    # [7]        -> ages 6 to 11
ages[(ages < 6) | (ages > 12)]     # [ 3  5 15]
ages[~(ages < 6)]                  # [ 7 12 15] -> everyone NOT under 6
```
Without the brackets, `ages >= 6 & ages < 12` fails with `ValueError: The truth value of an array ... is ambiguous`. That's because `&` runs before `>=`.

## Is It in a List? (`np.isin`)
Like `list.contains(x)` in Java, but for every item at once.
```python
states = np.array([6, 36, 48, 6, 12])
np.isin(states, [6, 36])           # [ True  True False  True False]
states[np.isin(states, [6, 36])]   # [ 6 36  6] -> keep only California (6) and New York (36)
```

## Sorting and Ranking
```python
rates = np.array([0.031, 0.045, 0.022])
names = np.array(["CA", "NY", "TX"])
np.sort(rates)                     # [0.022 0.031 0.045]
np.argsort(rates)                  # [2 0 1] -> the positions in sorted order, not the values
names[np.argsort(rates)[::-1]]     # ['NY' 'CA' 'TX'] -> names from highest to lowest rate
```
`argsort` keeps the names and rates lined up, which is what you need for "top states". `[::-1]` reverses the array.

## Rounding for Reports
```python
np.round(np.array([0.28571, 0.5]), 3)  # [0.286 0.5  ]
np.round(0.28571 * 100, 1)             # 28.6 -> as a percent
```

## Weighted Rate for Each Group
This is the main pattern for "rate by sex", "rate by age group" and so on. It combines a mask with the weighted average.
```python
has_autism = np.array([True, False, False, True])
weights    = np.array([100, 300, 200, 100])
sex        = np.array([1, 2, 1, 2])    # 1 = male, 2 = female

for code in np.unique(sex):
    mask = sex == code                 # pick this group's rows
    rate = np.average(has_autism[mask], weights=weights[mask])
    print(code, round(rate, 3))        # 1 0.333  then  2 0.25
```
pandas `groupby` will do this loop for you later, but it's good to see what it does underneath.

## Views vs Copies (A Java-Style Gotcha)
A slice is a **view**: it points at the same memory, like two Java variables pointing at the same array.
```python
x = np.array([1, 2, 3])
part = x[:2]
part[0] = 99
x                                  # [99  2  3] -> the original changed too!

x = np.array([1, 2, 3])
part = x[:2].copy()                # a real, separate copy
part[0] = 99
x                                  # [1 2 3] -> original is safe
```
Use `.copy()` when you plan to change a slice but want to keep the original.

## Random Numbers (for Practice Data)
```python
rng = np.random.default_rng(42)    # 42 is the seed -> same numbers every run, like new Random(42)
rng.integers(0, 18, size=5)        # [ 1 13 11  7  7] -> 5 fake ages from 0 to 17
```
Handy for testing code before the real survey file is loaded.
