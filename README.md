# Python 3 Notes
## Table of Contents
- [Dictionaries](#dictionaries)
  - [Iteration](#dictionary-iteration)
  - [Sorting](#dictionary-sorting)
- [Lists](#lists)
  - [Comprehensions](#list-comprehensions)
  - [Initialization](#list-initialization)
  - [Reversal](#list-reversal)
  - [Sorting](#list-sorting)
- [Strings](#strings)
  - [From List](#from-list)
  - [Python String Constants](#string-constants)
  - [`isalnum()`](#isalnum)
  - [`strip()`](#strip)
  - [`str()` vs `repr()`](#str-vs-repr)
- [Functional Iteration](#functional-iteration)
  - [`map()`](#map)
  - [`filter()`](#filter)
  - [`reduce()`](#reduce)
- [Decorators](#decorators)
  - [`@classmethod`](#classmethod)
  - [`@staticmethod`](#staticmethod)
  - [`@property`](#property)
- [Other Useful Built-in Functions](#built-in-functions)
  - [`abs()`](#abs)
  - [`any()`](#any)
  - [`all()`](#all)
  - [`chr()`](#chr)
  - [`enumerate()`](#enumerate)
  - [`input()`](#input)
  - [`isinstance()`](#isinstance)
  - [`len()`](#len)
  - [`max()`](#max)
  - [`min()`](#min)
  - [`ord()`](#ord)
  - [`pow()`](#pow)
  - [`type()`](#type) 
- [Common Gotchas](#common-gotchas)
  - [Nested List Initialization](#nested-list-initialization)
  - [Mutable Default Arguments](#mutable-default-arguments)

## Dictionaries
### Dictionary Iteration
Get w/ default value if key not in dict:
```python
my_dict[k] = my_dict.get(k, 0) + 1; # get retrieves value for k, or 0 if k not in dict
```

Iterating a dict iterates only the keys:
```python
for k in my_dict:  # k will be each key, not each key-value pair
    ...
```

Testing membership: `if k in dict: ...`

To get actual key-value pairs at the same time:
```python
for k,v in my_dict.items():
    ...
```
applies to comprehensions as well: `new_d = {k: v+1 for k,v in d.iteritems()}`

### Dictionary Sorting
It is not possible to sort a dictionary, only to get a representation of a dictionary that is sorted. Dictionaries are inherently orderless, but other types, such as lists and tuples, are not. So you need an ordered data type to represent sorted values, which will be a list—probably a list of tuples.  
- `sorted(d.iteritems())`
  - sorted list of key-value pairs by key
  - by value: `sorted(d.iteritems(), key=lambda x: x[1]`
- `sorted(d)`
  - sorted list of keys only
  - sorted list of keys by value: `sorted(d, key=lambda x: d[x])`

## Lists
### List Comprehensions
General Syntax:
```python
[<expression> for item in list if conditional]
```
is equivalent to:
```python
for item in list:
    if conditional:
        <expression>
```

Note how the order of the `for` and `if` statements remains the same.
For example, 
```python
for row in grid:
    for x in row:
        <expression>
```
is the same as
```python
[<expression> for row in grid for x in row]
```

### List Initialization
Can use comprehensions:
```python
my_list = [i for i in range(10)] # [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
```
2-D list (list of lists):
```python
my_list = [[] for i in range(3)] # [[], [], []]
```
This is useful for a "visited" grid of some kind (common in Dynamic Programming problems):
```python
visited = [[0 for i in range(len(grid[0]))] for j in range(len(grid))]
```
*BE CAREFUL* when initializing a matrix.
Do this:
```python
my_list = [[None] * n for i in range(n)]
```
**NOT** this:
```python
my_list = [[None] * n] * n
```
The latter method makes copies of the **reference** to the original list, thus any modification to one row will change the other rows in the same way. The first method does not do this.

A list can be created from a string using `list(my_str)`
We can apply a filter as well:
```python
my_list = list(c for c in my_str if c not in ('a', 'c', 'e'))
```

### List Reversal
 - `my_list[::-1]`
   - returns copy of list in reverse
 - `reversed(my_list)`
   - returns an iterator on the list in reverse
   - can turn into a list via `list(reversed(my_list))`
 - `my_list.reverse()`
   - actually modifies the list
 
### List Sorting
 - `sorted(my_list)`
   - returns copy of sorted list
 - `my_list.sort()`
   - actually modifies the list
   
 By default, these methods will sort the list in ascending order.
 For descending order, we can supply the arg `reverse=True` to either of the aforementioned methods.
 
 We can also override the key for sorting by supplying the `key` arg.
 For example, if we have a list of tuples and we want to use the second item as the key:
 ```python
 list1 = [(1, 2), (3, 3), (4, 1)] 
 list1.sort(key=lambda x: x[1]) # list1 is now [(4, 1), (1, 2), (3, 3)]
 ```
 Additionally, if we want to sort in descending order:
 ```python
 list1.sort(key=lambda x: x[1], reverse=True) # list1 is now [(3, 3), (1, 2), (4, 1)]
 ```
 
 When using `sorted()` it works the same, except we supply the list as the first arg:
 ```python
 list2 = sorted(list1, key=lambda x: x[1], reverse=True)
 ```
 
## Strings
### From List
```python
my_list = ['te', 's', 't', '1', '2', '3', '_']
s = ''.join(my_list) # "test123_"
s2 = ''.join(c for c in my_list if c.isalnum()) # "test123"
```

### String Constants
Python has a lot of useful string constants. A few of them are shown below.
For a complete list, see the [documentation](https://docs.python.org/3.7/library/string.html)
 - `string.ascii_letters`
 - `string.digits`
 - `string.whitespace`
 
 e.g. `if d in string.digits: ...`
 
### `isalnum()`
Returns `True` if a string consists only of alphanumeric characters.
```python
s = "test123"
s.isalnum() # True
```

### `strip()`
Returns copy of string without surrounding whitespace, if any.
```python
s = "   test "
s.strip() # returns "test"
```

### `str()` vs `repr()`
See [this GeeksForGeeks article](https://www.geeksforgeeks.org/str-vs-repr-in-python/) for more info.

## Functional Iteration
For some good explanations and examples for the following functions, see [here](http://book.pythontips.com/en/latest/map_filter.html).

Note that `map()` and `filter()` both return iterators, so if you want a list, you need to use `list()` on the output. However, this is typically better accomplished with list comprehensions or `for` loops for the sake of readability.
  
### `map()`
`map()` applies a function to all the items in a list.
```python
map(function_to_apply, list_of_inputs)
```
  
For example, the following code:
```python
items = [1, 2, 3, 4, 5]
squared = []
for i in items:
    squared.append(i**2)
```
can be accomplished more easily with `map()`:
```python3
items = [1, 2, 3, 4, 5]
squared = list(map(lambda x: x**2, items))
```

### `filter()`
`filter()` creates a list of elements for which a function returns `True`.
  
Here's an example:
```python3
number_list = range(-5, 5)
less_than_zero = list(filter(lambda x: x < 0, number_list))
print(less_than_zero) # [-5, -4, -3, -2, -1]
```

### `reduce()`
`reduce()` is used to perform a rolling computation on a list.

Here's an example:
```python3
from functools import reduce
number_list = [1, 2, 3, 4]
product = reduce((lambda x, y: x * y), number_list) # output: 24
```

Often times, an explicit `for` loop is more readable than using `reduce()`.
But if you're trying to flex in an interview, and the problem calls for it, it could be a nice way to subtly show your understanding of functional programming.

## Decorators
A decorator is a function returning another function, usually applied as a function transformation using the `@wrapper` syntax. This syntax is merely syntactic sugar.

The following two function definitions are semantically equivalent:
```python3
def f(...):
    ...
f = staticmethod(f)

@staticmethod
def f(...):
    ...
```

### @classmethod
Transform a method into a class method. A class method receives the class as implicit first argument, just like how an instance method receives the instance. To declare a class method:
```python3
class C:
    @classmethod
    def f(cls, arg1, arg2, ...):
        ...
```

A class method can be called either on the class (like `C.f()`) or on an instance (like `C().f()`). The instance is ignored except for its class. If a class method is called for a derived class, the derived class object is passed as the implied first argument.

Note that class methods are not the same as C++ or Java static methods. If you want those, see [`@staticmethod`](#staticmethod).

### @staticmethod
Transform a method into a static method. A static method does not receive an implicit first argument. To declare a static method:
```python3
class C:
    @staticmethod
    def f(arg1, arg2, ...):
        ...
```

A static method can be called either on the class (like `C.f()`) or on an instance (like `C().f()`). Static methods in Python are similar to those found in Java or C++.

### @property
Return a property attribute.
Usage:
```python3
property(fget=None, fset=None, fdel=None, doc=None)
```
`fget` is a function for getting an attribute value. `fset` is a function for setting an attribute value. `fdel` is a function for deleting an attribute value. `doc` creates a docstring for the attribute.

The following is a typical use case for defining a managed attribute `x`:
```python3
class C:
    def __init__(self):
        self._x = None

    def getx(self):
        return self._x

    def setx(self, value):
        self._x = value

    def delx(self):
        del self._x

    x = property(getx, setx, delx, "I'm the 'x' property.")
```
Or, equivalently:
```python3
class C:
    def __init__(self):
        self._x = None

    @property
    def x(self):
        """I'm the 'x' property."""
        return self._x

    @x.setter
    def x(self, value):
        self._x = value

    @x.deleter
    def x(self):
        del self._x
```    

If `c` is an instance of `C`, then `c.x` will invoke the getter; `c.x = value` will invoke the setter; and `del c.x` the deleter.

If `doc` is not provided, the property will copy `fget`'s docstring, if it exists. Thus, it is straightforward to create read-only properties with the `@property` decorator:
```python3
class Parrot:
    def __init__(self):
        self._voltage = 100000

    @property
    def voltage(self):
        """Get the current voltage."""
        return self._voltage
```
The `@property` decorator turns the `voltage()` method into a “getter” for a read-only attribute with the same name, and it sets the docstring for `voltage` to “Get the current voltage.”

For more information, check out [the documentation](https://docs.python.org/3/library/functions.html#property) and [this Programiz article](https://www.programiz.com/python-programming/property).

## Built-in Functions
For a complete list of built-ins in Python 3, see [the documentation](https://docs.python.org/3/library/functions.html).
### `abs()`
Returns the absolute value of a number, either an integer or floating point number.
If the argument is a complex number, its magnitude is returned.

### `any()`
Usage:
```python
any(iterable)
```
`any()` takes any iterable as an argument and returns `True` if at least one element of the iterable is `True`.

```python
any([1, 3, 4, 0])   # True
any([0, False])     # False
any([0, False, 5])  # True
any([])             # False

any("This is good") # True
any("0")            # True
any("")             # False
```

See [here](https://www.programiz.com/python-programming/methods/built-in/any) for more info.

Check if any tuples contain a negative value:
```python
if any(x < 0 or y < 0 for (x, y) in list_ranges): ...
```

### `all()`
```python
all(iterable)
```
`all()` takes any iterable as an argument and returns `True` if all the elements of the iterable are `True`.

```python
all([1, 3, 4, 5])  # True
all([0, False])    # False
all([1, 3, 4, 0])  # False
all([0, False, 5]) # False
all([])            # True

all("This is good") # True
all("0")            # True
all("")             # True
```

See [here](https://www.programiz.com/python-programming/methods/built-in/all) for more info.

Check if all elements of a list are `x`: 
```python
if all(c == x for c in alst): ...
```

### `chr()`
Returns the string representing a character whose Unicode code point is the integer passed.

For example, `chr(97)` returns the string `a`, while `chr(8364)` returns the string `€`.

This is the inverse of [`ord()`](#ord).

### `enumerate()`
Usage:
```python3
enumerate(iterable, start=0)
```
Returns an enumerate object. `iterable` must be a sequence, iterator, or some object which suports iteration. The `__next__()` method of the iterator returned by `enumerate()` returns a tuple containing a count (from `start` which defaults to zero) and the values obtained from iterating over the iterable.  

Example:
```python3
seasons = ['Spring', 'Summer', 'Fall', 'Winter']
list(enumerate(seasons))              # [(0, 'Spring'), (1, 'Summer'), (2, 'Fall'), (3, 'Winter')]
list(enumerate(seasons, start=1))     # [(1, 'Spring'), (2, 'Summer'), (3, 'Fall'), (4, 'Winter')]
```
This is equivalent to:
```python3
def enumerate(sequence, start=0):
    n = start
    for elem in sequence:
        yield n, elem
        n += 1
```

### `input()`
Gets input from the user.
Usage:
```python3
input([prompt])
```

Example: 
```python3
>>> s = input('-> ')  
-> Monty Python's Flying Circus
>>> s  
"Monty Python's Flying Circus"
```
If the `prompt` arg is present, it is written to stdout without a trailing newline.

### `isinstance()`
Usage:
```python3
isinstance(object, classinfo)
```

Returns true if the `object` argument is an instance of the `classinfo` argument, or of a (direct, indirect, or virtual) subclass thereof. Returns false otherwise.

If `classinfo` is a tuple of type objects, return true if `object` is an instance of any of *any* of these types.

### `len()`
Return the length of an object. The argument may be a sequence (e.g. string, bytes, tuple, list, or range) or a collection (e.g. dictionary, set, frozen set).

### `max()`
Returns the max item in an iterable, or the max of multiple arguments passed.

### `min()`
Returns the min item in an iterable, or the min of multiple arguments passed.

### `ord()`
Given a string representing one Unicode character, return an integer representing the Unicode code point of that character.

For example, `ord('a')` returns the integer 97. `ord('€')` (Euro sign) return 8364. 

This is the inverse of [`chr()`](#chr).

### `pow()`
Usage:
```python3
pow(x, y[, z])
```
Return `x` to the power `y`; if `z` is present, return `x` to the power `y`, modulo `z` (computed more efficiently than `pow(x, y) % z`).  
`pow(x, y)` is equivalent to `x**y`.

### `type()`
Usage:
```python3
type(object)
type(name, bases, dict)
```

With one argument, return the type of `object`. The return value is a type object and generally the same object as returned by `object.__class__`.

E.g.
```python3
x = 5
type(x)  # class 'int'
```

The [`isinstance()`](#isinstance) function is recommended for testing the type of an object, since it accounts for subclasses.

## Common Gotchas
### Nested List Initialization
When creating a list of lists, be sure to use the following structure:
```python
my_list = [[None] * n for i in range(n)]
```
Read the section on [list initialization](#list-initialization) to see why.

### Mutable Default Arguments
If we try to do something like `def f(x, arr=[])` this will most likely create undesirable behavior.
Default arguments are resolved *only once*, when the function is first defined. The same arg will be used in successive function calls. In the case of a mutable type like a list, this means that changes made to the list in one call will be carried over in successive calls.
Instead, consider doing:
```python
def f(x, arr=None):
    if not arr: arr = []
```
