# NumPy Learnings

## Resources
https://numpy.org/devdocs/user/quickstart.html

## Java Comparison
- A NumPy array (`np.array`) is like a Java `double[]`: fixed size, one type, fast.
- Math works on the whole array at once. `arr * 2` doubles every item, with no `for` loop needed.
- `np.average(values, weights=w)` is like looping over two arrays in Java, summing `value * weight`, then dividing by the sum of the weights.
