# Python's functions are objects
Or, why this behaviour makes really sense:

```python
def f(val, nums = []) -> list:
  nums.append(val)
  return nums

f(1)
assert f(2) == [1, 2]
```

The rationale is that a function is _a first-class object_ (see below). The value is created at the definition when the interpreter encounters the `def` keyword. Here the object is bound to the name `f`, and the arguments as well.

In other terms, the function above is phenomenologically equivalent to an instance of such a class:

```python
class f(object):

  def __init__(self):
    self.nums = []

  def __call__(self, val, _nums = None):
    if _nums:
      _nums.append(val)
      return _nums
    self.nums.append(val)
    return self.nums
```

This works as follows:
1. at `def f(...):` the member `nums` is initialised to an empty list, that is the default value for the argument
2. at the first invocation without providing the second argument, equivalent to invoking the method `__call__(val)` the attribute `nums` is updated
3. at the second invocation still without providing the second argument, the attribute `nums` is again updated resulting in a list containing two values
4. when the function is invoked by providing a list as second argument, this one is employed irrespective of the member attribute.

As a result, it holds:

```python
a = f()
a(5)
assert a(6) == [5, 6]

b = f()
assert b(4, [0]) == [0, 4]
assert a(1) == [5, 6, 1]
```

### A cautionary tale on default arguments for mutables
A list is a mutable. The behaviour described in this document, puzzling to whom not acquainted with Python's logic, should suggest the programmer to adopt another approach when defining default values for mutable arguments.

Alternatives are:
- the `None` value
- a _sentinel_ object such as `object()` (an instance of the primitive class) 

### Def: first-class citizens
A possible definition is that a first-class object can be:
- assigned to a variable
- passed as argument of a function
- returned by a function
- tested for equality

In case of a first-class function, this can be passed as an argument of _higher-order_ functions. This is necessary to functional programming.

Moreover, a first-class function allows for the definition of a _closure_ that is the following structure:

```python
def f(x):
  def g():
    return 1+x
  return g

assert f(4)() == 5
```
