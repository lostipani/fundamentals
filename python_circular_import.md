# Circular importing
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

### This fails
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

### This works
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

### But this fails
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
