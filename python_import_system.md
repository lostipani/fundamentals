# Import system

## Resolution
Python follows a `LEGB` resolution rule
1. Local (inside a function)
2. Enclosing (`nonlocal` in nested function)
3. Global (module's scope)
4. Built-in (`len`, `str`, `next`, ...)

#### Bonus: when using `nonlocal`
For instance, in case of a list manipulation, the method `extend` has Enclosing scope resolution, whereas the augmented assignment `+=` only Local.
```
def main():
  x = [1, 2]
  y = x[:] # this makes a copy of the list, instead y = x would just reference the same object!

  def foo():
    x.extend([3, 4])
  def goo():
    nonlocal y
    y += [3, 4]

  foo()
  assert x == [1, 2, 3, 4]
  goo()
  assert y == [1, 2, 3, 4]
```

## Import workflow
When typing `from mod import foo` or simpy `import mod` the following happens
1. `mod` is executed (global statements executed, functions and classes defined)
2. the module is added to `sys.modules[mod]`
3. names such as `foo` in the caller's namespace (see `__dict__`) are bound to the object imported from `sys.modules[mod].__dict__`

## Circular import resolution
Say you have a package
```
package/
├── a.py
├── b.py
└── __init__.py
```
and modules are run as follows
```
python -m package.a
python -m package.b
```

#### This fails
File `/a.py`
```python title="/a.py"
from package.b import goo  # global (= module's scope) referencing of name goo

def foo():
    return goo()+1
```
File `/b.py`
```python title="/b.py"
import package.a

def goo():
    return 1
```
* Therefore importing with the `from <module> import <object>` statement helps catch circular dependencies.

#### This works
```python title="/a.py"
import package.b as b

def foo():
    return b.goo()+1 # local referencing of name goo
```
File `/b.py`
```python title="/b.py"
import package.a

def goo():
    return 1
```

#### But this fails
File `/a.py`
```python title="/a.py"
import package.b as b

class A(b.B):
    pass
```
File `/b.py`
```python title="/b.py"
import package.a as a

class B(object):
    pass
```
This because `b.B` as parent class of `A` is referenced at module's level.
