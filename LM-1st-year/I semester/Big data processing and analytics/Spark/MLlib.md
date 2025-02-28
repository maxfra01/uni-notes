# Spark MLlib

This library is based on a set of basic local and distributed **data types**:
- Local vector
- Labeled point
- Local matrix
- Distributed matrix

### Local vectors

Used to represent a vectors of double objects. There is a specific class to do this: 2 mode are available, **dense** or **sparse** representation.
Consider the vector (1.0, 0.0, 3.0), then its dense representation is the vector itself: (1.0, 0.0, 3.0) while the sparse one is the combination of three part: the number of elements, the list of non-zero elements indexes and the list of non zero values: (3, \[0, 2\], \[1.0, 3.0\]).

### Labeled point

It is used to represent a vector of double and a single double value.
A label is an integer number representing the class we predict (range of label is from 0 to C-1, where C is the number of distinct class)
