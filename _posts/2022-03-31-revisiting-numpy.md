---
title: Revisiting NumPy
tags:
- review
- learning
- numpy
- handbook
- JakeVanderPlas
desc: revisiting numpy
layout: post
---
Now, taking a look at the NumPy chapter from Jake VanderPlas book "[Python Data
Science
Handbook](https://jakevdp.github.io/PythonDataScienceHandbook/02.00-introduction-to-numpy.html)".
For me, it is more like a revisit to NumPy.
<!-- more -->
Python dynamic typing refers to the ability of the language to infer the
variable type. Numpy arrays are implemented like C variables: a single pointer
to a position in memory and a few other information. Therefore, Python's lists
are more flexible while NumPy arrays are more efficient to store and manipulate
homogeneus data.

NumPy arrays, or matrices, can be created from:

```python
>>> np.ndarray([1, 3, 4, 5]) # Python Lists
# From built-in routines
>>> np.zeros(10, dtype='int') 
>>> np.ones((3, 5), dtype='int') # A 3x5 identity matrix
>>> np.full(3, (4, 4), dtype='int') # A 4x4 matrix of 3's
```

ndarrays will upcast the type for the type of the largest element.

```python
>>> np.ndarray([3.14, 4, 3, 2])
# Upcast ALL elements to float
>>> array([3.14, 4.  , 2.  , 3.  ])
```

And inserting a data that is larger than the array type will result into truncation:

```python
>>> arr = np.ndarray([3, 4, 3, 2])
>>> arr[0] = 2.71828  # This will be truncated
>>> array([2, 4, 2, 3])
```

Slicing in NumPy returns views, that is a reference to that portion in the
memory. On the other hand, Python list slicing returns a copy of the array.
Therefore, changes to a NumPy array slice will also change the original array.


```python
>>> mat = np.full((3, 3), 2, dtype='int16')
>>> mat_view = mat[:2, :2]
>>> mat_view[0, 0] = 42
>>> mat
>>> array([[42,  2,  2],
           [ 2,  2,  2],
           [ 2,  2,  2]], dtype=int16)
```

Interestingly, numpy's *np.log* computes the natural logarithm ($$\ln$$) and
not the $$\log_{10}$$. This can be confusing, while it is better to find when
looking in the interface, it is more logical to think it is using base 10.


### Broadcasting
Furthermore, the array broadcasting works by three rules:

> 1. if the two arrays differ in the number of dimensions, the shape of the one
>    with the fewer dimensions is padded with ones in its leading (left) side;
> 2. if the shape of the two arrays does not match in any dimension, the array
>    with shape equal to 1 in that dimension is stretched to match the other
>    shape;
> 3. if in any dimension the sizes disagree and neither is equal to 1, an error
>    is raised.

For example, suppose that A.shape = (2, 3) and B.shape = (3,) from *Rule 1*, B
is padded in the left resulting into B.shape = (1, 3). Then, *Rule 2* is
applied on the first dimension since they disagree. Thus, B.shape = (2, 3) and
the operation results into an array C with shape (2, 3). The rules are applied
only once, so that if B.shape was (2,), after applying *Rule 1* and *Rule 2*
B.shape = (2, 2) and it would disagree with A.shape resulting into an error by
*Rule 3*.


