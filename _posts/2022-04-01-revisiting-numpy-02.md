---
title: Revisiting NumPy [2]
tags:
- review
- learning
- numpy
- handbook
- JakeVanderPlas
desc: revisiting numpy
layout: post
---
This is a continuation from the [previous post]({% post_url
/2022-03-31-revisiting-numpy-01 %}) where I explore the NumPy chapter from Jake
VanderPlas book "[Python Data Science
Handbook](https://jakevdp.github.io/PythonDataScienceHandbook/02.00-introduction-to-numpy.html)".
<!-- more -->

### Comparison, booleans and Masking
In this follow-up reading, the most relevant was the explanation on using
boolean operators and built-in NumPy's *ufuncs*. Using them will results into
an boolean array, and they can be used for finding zeros, NaN's, counting
negative values per column, and many other applications. Also, they can be used
with **bitwise** operators. These are overloaded in NumPy for element-wise
boolean operations.

```python
>>> x = np.random.randint(10, (3, 4))
# Count how many elements are lower than -3
>>> np.count_nonzero(x < 3)
# Equivalent of the above
>>> np.sum(x < 3)
# Elements lower than 8 in each column
>>> np.count_nonzero(x < 8, axis=1)
# Number of elements between 0 and 3
>>> np.count_nonzero((x < 3) & (x > 0))
```
- **&**: bitwise_AND
- **~**: bitwise_NOT
- **^**: bitwise_XOR
- **\|**: bitwise_OR

However, these operations really shiny when used for filtering of **masking**.
That is a way of using boolean arrays as indexes, where the result is all the
elements in from True indexes.

```python
>>> x
>>> array([5, 0, 3, 3])
>>> x[x < 5]
>>> array([0, 3, 3])
```

### Fancy Indexing
Fancy indexing is choosing several elements from an array in a possible
unordered way. That is, you can kind of cherry-pick elements. For example, instead of using this
```python
>>> x = np.random.randint(100, size=10)
>>> [x[1], x[7], x[4]]
```
we can do That
```python
>>> ind = [1, 7, 4]
>>> x[ind]
```
and even reshape, since the result is always in the shape of the indexes array,
rather than the values array.
```python
>>> ind = [[1, 7], 
           [4, 3]]
>>> x[ind]
>>> # 2x2 matrix
```
This can also be used to split datasets into training, validation and test.

Another useful information in the book is about the Structured Arrays.
Sincerely, I always thought that was a Pandas-only implementation, but I was
surprised to see that Pandas had it built upon NumPy's structured arrays.
These structured allow you to create heterogeneous arrays in NumPy. Allowing
indexing by names, just like Pandas' DataFrames.

