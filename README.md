## Table of Contents
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
  - [`str()` vs `repr`](#str-vs-repr)

  
- [Common Gotchas](#common-gotchas)
  - [Nested List Initialization](#nested-list-initialization)
  - [Mutable Default Arguments](#mutable-default-arguments)

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
