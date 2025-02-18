# Numpy

We need to import **numpy** in every module.

```python
import numpy as np
```

### Array: create, modify

Main objects in numpy are **multidimensional arrays** `ndarray`: we'll use 1-dimension and 2-dimension arrays (NOT `matrix`).
We can create arrays by using numpy methods, or build arrays from lists and tuples:

```python
a = numpy.array([1,2,3])

b= numpy.array([[1,2,3], [4,5,6]], dtype=numpy.float64) #add the type

c= numpy.array((2,5,3))
```

To retrive information of arrays:

```python
ndarray.size #total number of elements
ndarray.shape #array's shape in format (n,m)
ndarray.dim #number of axis
ndarray.dtype #type of elements
```

Numpy allows to create particular arrays using special functions:

```python
>>> numpy.zeros((2, 3), dtype=numpy.float32) #return a zero matrix

>>> numpy.ones(5) #return a matrix full of ones

>>> numpy.arange(0, 4, 0.5) #same logic as range, it return 1-dimension array

>>> numpy.eye(3) #identity matrix

>>> numpy.linspace(0, 5, 4) #4 evenly spaced items in 0-5 range
```

The arithmetic operators are **element-wise operations** (no matrix multiplication): to multiply two matrix in the correct way we can use the `dot` method:

```python
>>> x = numpy.array([[1,2], [3,4], [5,6]])
>>> y = numpy.array([[1,2,3], [4,5,6]])
>>> z = numpy.dot(x, y)
```

Note that dimensions of arrays should match.

We can edit the shape of an array using the `reshape` method or the `ravel` method:

```python
>>> x = numpy.arange(12).reshape((2,3)) #it returns a 2x3 matrix 

>>> y = x.ravel() #returns a monodimensional array
```

```ad-note
title: Row vectors
Row vectors (shape (1, n)) are NOT 1-dimensional arrays (shape(n,) ).
We will represent data as **column vector**: matrix will be composed by horizontally stacked column vectors. 
```

To **transpose** a matrix we can use the `.T` attribute.

### Slicing arrays

Slicing an array can be performed in the same way of python:

```python
>>> x = numpy.arange(5)

>>> x[1:3]
array([1, 2])

>>> x[::2]
array([0, 2, 4])

>>> x[3]
3
```

For multidimensional arrays we can specify intervals for each axis:

```python
>>> x
array([[ 0, 1, 2, 3, 4],
		[ 5, 6, 7, 8, 9],
		[10, 11, 12, 13, 14]])
		
>>> x[1, 0:3]
array([5, 6, 7])
```

We can slice arrays using indexes inside two arrays (idx, jdx), using the `ix_` method from numpy:

```python
>>> idx = numpy.array([0, 2])
>>> jdx = numpy.array([1, 3])

>>> x[numpy.ix_(idx, jdx)]
array([[ 1, 3],
		[11, 13]])
```

We can also use **bool arrays** to perform slicing.

In general, slicing creates array views: A view is an array that shares its data with a different one. We can also create explicit views:  note that modification to a view modifies the original array.
That's because slicing an array return an array that does not **own his data**: to check if a vector owns his data we can use `.owndata()` method.
To create a real copy we can use the `copy()` method.

### Broadcasting

The term broadcasting describes how NumPy treats arrays with different shapes during arithmetic operations. Subject to certain constraints, the smaller array is “broadcast” across the larger array so that they have compatible shapes.

![[Pasted image 20240314153145.png]]

### Concatenate arrays

We can use the `concatenate()` or `hstack()`/`vstack()` methods:

```python
>>> x1 = numpy.array([[1,2,3]])
>>> x2 = numpy.array([[4,5,6]])
>>> numpy.hstack([x1, x2])
array([[1, 2, 3, 4, 5, 6]])

>>> numpy.vstack([x1, x2])
array([[1, 2, 3],
	   [4, 5, 6]])
```

### Other operators

Many methods are available: `sum()`, `max()`...
Inside the `linalg` module there are many methods to manipulate matrix.


# Matplotlib

It's a popular library to plot functions and data. First of all we need to import the main module (pyplot) inside a python file:

```python
import matplotlib.pyplot as plt
```

### Plot a function

```python
import matplotlib.pyplot as plt
x = numpy.linspace(0, 5, 1000)
plt.plot(x, numpy.sin(x))
plt.xlabel(’X-Axis’)
plt.ylabel(’Y-Axis’)
plt.show()
```

### Plot 2-D data

We can use scatter plots:

```python
import matplotlib.pyplot as plt
D = numpy.random.random((2, 100))
plt.scatter(D[0], D[1])
plt.show()
```

### Plot histogram 

We can also plot histograms and combine different plots: in order to do that we just need to call the `show()` method after adding all elements:

```python
import matplotlib.pyplot as plt
D = numpy.random.normal(size=1000)
plt.hist(D, bins = 20, density=True, ec=’black’, color=’800000’, alpha = 0.5)
x = numpy.linspace(-4, 4, 1000)
y = 1.0/(2*numpy.pi)**0.5 * numpy.exp(-0.5 * x**2)
plt.plot(x, y, color=(0.0, 0.2, 0.8), linewidth=4)
plt.show()
```

![[Pasted image 20240314154354.png]]

Next: [[Classification]]