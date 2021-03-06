### Sources
- [Array manipulation routines](https://docs.scipy.org/doc/numpy-1.15.0/reference/routines.array-manipulation.html)


#### Array manipulation routines
##### Basic operations
- copyto(dst, src[, casting, where])	Copies values from one array to another, broadcasting as necessary.

``` python
# numpy.copyto(dst, src, casting='same_kind', where=True)
# Copies values from one array to another, broadcasting as necessary.

import numpy as np

a = np.arange(6, dtype='f4').reshape(2,3)
d = np.arange(6,dtype='f4').reshape(2,3)
np.copyto(d,a)
```

##### Changing array shape
- reshape(a, newshape[, order])	Gives a new shape to an array without changing its data.
- ravel(a[, order])	Return a contiguous flattened array.

flatten always returns a copy.

ravel returns a view of the original array whenever possible. 

``` python
x = np.array([[1, 2, 3], [4, 5, 6]])
print(np.ravel(x))

#result: [1 2 3 4 5 6]
```

- ndarray.flat	A 1-D iterator over the array.
- ndarray.flatten([order])	Return a copy of the array collapsed into one dimension.

##### Transpose-like operations
- moveaxis(a, source, destination)	Move axes of an array to new positions.

[check this link](https://stackoverflow.com/questions/42312670/how-does-numpy-swapaxes-work)

``` python
import numpy as np

x = np.arange(9).reshape(3,3)
print(x)

#array([[0, 1, 2],
#       [3, 4, 5],
#       [6, 7, 8]])

print(np.moveaxis(x,0,1))
#array([[0, 3, 6],
#       [1, 4, 7],
#       [2, 5, 8]])

# create an array contains 2 objects and each object has 3 by 4 matrices
y = np.zeros((2,3,4))
print(y.shape)
#result (2,3,4)

print(np.moveaxis(y,0,1).shape)
#result (3,2,4)

```
- rollaxis(a, axis[, start])	Roll the specified axis backwards, until it lies in a given position.
``` python
a = np.ones((3,4,5,6))
np.rollaxis(a, 3, 1).shape
# result (3, 6, 4, 5)
np.rollaxis(a, 2).shape
# result (5, 3, 4, 6)
np.rollaxis(a, 1, 4).shape
# result (3, 5, 6, 4)

```

- swapaxes(a, axis1, axis2)	Interchange two axes of an array.
- ndarray.T	Same as self.transpose(), except that self is returned if self.ndim < 2.
- transpose(a[, axes])	Permute the dimensions of an array.

##### Changing number of dimensions
- atleast_1d(*arys)	Convert inputs to arrays with at least one dimension.
- atleast_2d(*arys)	View inputs as arrays with at least two dimensions.
- atleast_3d(*arys)	View inputs as arrays with at least three dimensions.

``` python
g = np.atleast_1d(3)
gg = np.atleast_2d(3)
ggg = np.atleast_3d(3)
print(g)
#array([3])
print(gg)
#array([[3]])
print(ggg)
#array([[[3]]])

```

- broadcast	Produce an object that mimics broadcasting.

``` python

import numpy as np
x = np.array([[1], [2], [3]])
y = np.array([4, 5, 6])
b = np.broadcast(x, y)
print(b.shape)
# result (3,3)
row,col= b.iters
print(row)

#result: 
#array([[1],
#       [2],
#       [3]])

print(col.base)
# result : [4 5 6]

```

- broadcast_to(array, shape[, subok])	Broadcast an array to a new shape.
- broadcast_arrays(*args, **kwargs)	Broadcast any number of arrays against each other.
- expand_dims(a, axis)	Expand the shape of an array.

``` python 

import numpy as np

x = np.array([1,2])
x.shape
#result : (2,)


y = np.expand_dims(x, axis=0)
print(y)
#result . array([[1, 2]])
y.shape
#result : (1, 2)

y = np.expand_dims(x, axis=1)  # Equivalent to x[:,np.newaxis]
print(y)
# result : array([[1],
#          [2]])
y.shape
#result : (2, 1)

```


- squeeze(a[, axis])	Remove single-dimensional entries from the shape of an array.


``` python
import numpy as np

c = np.arange(16).reshape(4,2,2)
print(c.shape)
# result: (4,2,2)

d = np.expand_dims(c, axis=0)

print(d.shape)

# result : (1,4,2,2)

d = np.squeeze(d)

print(d.shape)

# result : (4,2,2)

```







##### Changing kind of array
- asarray(a[, dtype, order])	Convert the input to an array.
- asanyarray(a[, dtype, order])	Convert the input to an ndarray, but pass ndarray subclasses through.
- asmatrix(data[, dtype])	Interpret the input as a matrix.
- asfarray(a[, dtype])	Return an array converted to a float type.
- asfortranarray(a[, dtype])	Return an array laid out in Fortran order in memory.
- ascontiguousarray(a[, dtype])	Return a contiguous array in memory (C order).
- asarray_chkfinite(a[, dtype, order])	Convert the input to an array, checking for NaNs or Infs.
- asscalar(a)	Convert an array of size 1 to its scalar equivalent.
- require(a[, dtype, requirements])	Return an ndarray of the provided type that satisfies requirements.

This function is useful to be sure that an array with the correct flags is returned for passing to compiled code (perhaps through ctypes).

    - requirements : str or list of str
        The requirements list can be any of the following

        ‘F_CONTIGUOUS’ (‘F’) - ensure a Fortran-contiguous array
        ‘C_CONTIGUOUS’ (‘C’) - ensure a C-contiguous array
        ‘ALIGNED’ (‘A’) - ensure a data-type aligned array
        ‘WRITEABLE’ (‘W’) - ensure a writable array
        ‘OWNDATA’ (‘O’) - ensure an array that owns its own data
        ‘ENSUREARRAY’, (‘E’) - ensure a base array, instead of a subclass

``` python
import numpy as np
x = np.arange(6).reshape(2,3)
x.flags
# Result :
  #C_CONTIGUOUS : True
  #F_CONTIGUOUS : False
  #OWNDATA : False
  #WRITEABLE : True
  #ALIGNED : True
  #WRITEBACKIFCOPY : False
  #UPDATEIFCOPY : False
y = np.require(x, dtype=np.float32, requirements=['A', 'O', 'W', 'F'])
y.flags

# result : 

  #C_CONTIGUOUS : False
  #F_CONTIGUOUS : True
  #OWNDATA : True
  #WRITEABLE : True
  #ALIGNED : True
  #WRITEBACKIFCOPY : False
  #UPDATEIFCOPY : False



```


##### Joining arrays
- concatenate((a1, a2, …)[, axis, out])	Join a sequence of arrays along an existing axis.
- stack(arrays[, axis, out])	Join a sequence of arrays along a new axis.

``` python
arrays = [np.random.randn(3, 4) for _ in range(10)]
np.stack(arrays, axis=0).shape
# result. (10, 3, 4)

np.stack(arrays, axis=1).shape
# result : (3, 10, 4)


np.stack(arrays, axis=2).shape
#result : (3, 4, 10)

```



- column_stack(tup)	Stack 1-D arrays as columns into a 2-D array.

``` python
import numpy as np
a = np.array((1,2,3))
b = np.array((2,3,4))
np.column_stack((a,b))

#result : array([[1, 2],
#                [2, 3],
#                [3, 4]])

```


- dstack(tup)	Stack arrays in sequence depth wise (along third axis).

``` python
a = np.array((1,2,3))
b = np.array((2,3,4))
np.dstack((a,b))
# result: array([[[1, 2],
#        [2, 3],
#        [3, 4]]])

a = np.array([[1],[2],[3]])
b = np.array([[2],[3],[4]])
np.dstack((a,b))
#result :array([[[1, 2]],
#       [[2, 3]],
#       [[3, 4]]])

```

- hstack(tup)	Stack arrays in sequence horizontally (column wise).
- vstack(tup)	Stack arrays in sequence vertically (row wise).
- block(arrays)	Assemble an nd-array from nested lists of blocks.

##### Splitting arrays
- split(ary, indices_or_sections[, axis])	Split an array into multiple sub-arrays.

``` python
import numpy as np

x = np.arange(9.0)
np.split(x, 3)
# result : [array([ 0.,  1.,  2.]), array([ 3.,  4.,  5.]), array([ 6.,  7.,  8.])]

x = np.arange(8.0)
np.split(x, [3, 5, 6, 10])
# result : [array([ 0.,  1.,  2.]),
#          array([ 3.,  4.]),
#          array([ 5.]),
#          array([ 6.,  7.]),
#          array([], dtype=float64)]




```



- array_split(ary, indices_or_sections[, axis])	Split an array into multiple sub-arrays.
- dsplit(ary, indices_or_sections)	Split array into multiple sub-arrays along the 3rd axis (depth).
- hsplit(ary, indices_or_sections)	Split an array into multiple sub-arrays horizontally (column-wise).
- vsplit(ary, indices_or_sections)	Split an array into multiple sub-arrays vertically (row-wise).

##### Tiling arrays
- tile(A, reps)	Construct an array by repeating A the number of times given by reps.
- repeat(a, repeats[, axis])	Repeat elements of an array.

##### Adding and removing elements
- delete(arr, obj[, axis])	Return a new array with sub-arrays along an axis deleted.
- insert(arr, obj, values[, axis])	Insert values along the given axis before the given indices.
- append(arr, values[, axis])	Append values to the end of an array.
- resize(a, new_shape)	Return a new array with the specified shape.
- trim_zeros(filt[, trim])	Trim the leading and/or trailing zeros from a 1-D array or sequence.
- unique(ar[, return_index, return_inverse, …])	Find the unique elements of an array.

##### Rearranging elements
- flip(m[, axis])	Reverse the order of elements in an array along the given axis.
Reverse the order of elements in an array along the given axis.

The shape of the array is preserved, but the elements are reordered.

Parameters:	
m : array_like - Input array.
axis : None or int or tuple of ints, optional
Axis or axes along which to flip over. The default, axis=None, will flip over all of the axes of the input array. If axis is negative it counts from the last to the first axis.

If axis is a tuple of ints, flipping is performed on all of the axes specified in the tuple.

Changed in version 1.15.0: None and tuples of axes are supported

Returns: out : array_like - A view of m with the entries of axis reversed. Since a view is returned, this operation is done in constant time.

----

Notes

- flip(m, 0) is equivalent to flipud(m).

- flip(m, 1) is equivalent to fliplr(m).

- flip(m, n) corresponds to m[...,::-1,...] with ::-1 at position n.

- flip(m) corresponds to m[::-1,::-1,...,::-1] with ::-1 at all positions.

- flip(m, (0, 1)) corresponds to m[::-1,::-1,...] with ::-1 at position 0 and position 1.





``` python
A = np.arange(8).reshape((2,2,2))
print(A)
#result : array([[[0, 1],
#        [2, 3]],
#       [[4, 5],
#        [6, 7]]])
print(flip(A, 0))
# result : array([[[4, 5],
#        [6, 7]],
#       [[0, 1],
#        [2, 3]]])
print(flip(A, 1))
#result : array([[[2, 3],
#        [0, 1]],
#       [[6, 7],
#        [4, 5]]])
print(np.flip(A))
#array([[[7, 6],
#        [5, 4]],
#       [[3, 2],
#        [1, 0]]])
print(np.flip(A, (0, 2)))
#array([[[5, 4],
#        [7, 6]],
#       [[1, 0],
#        [3, 2]]])
A = np.random.randn(3,4,5)
np.all(flip(A,2) == A[:,:,::-1,...])
#result : True

```



- fliplr(m)	Flip array in the left/right direction.
- flipud(m)	Flip array in the up/down direction.
- reshape(a, newshape[, order])	Gives a new shape to an array without changing its data.
- roll(a, shift[, axis])	Roll array elements along a given axis.
- rot90(m[, k, axes])	Rotate an array by 90 degrees in the plane specified by axes.


