# Class inheritance
Python automatically resolves the order of methods of a multiple inheritance chain. This is called `mro` (Method Resolution Order) and it's stored as an attribute of any class and instance.

To let the magic happens, the function `super` should be invoked from each class' method. Even tough classes `A` and `B` are not in direct relation, the `A`'s method `quack` invokes the `super`.
```
class A(object):
  def quack():
    super().quack()
    
class B(object):
  def quack():
    print("quaack")

class C(A, B):
  def quack():
    super().quack()
```

The `mro` of the example above is
```
>>> C.mro()
[<class '__main__.C'>, <class '__main__.A'>, <class '__main__.B'>, <class 'object'>
```
