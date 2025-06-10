# Metaclasses
Since in Python everything is an object, classes are to be created by other kind of objects, namely metaclasses. This means that when the `class` keyword is encountered during code parsing, a metaclass is called. The default metaclass is `type` which acts as a factory for classes:
```
class A(BaseClass):
  _attribute = True
```
is equivalent to
```
A = type('A', (BaseClass,), {"_attribute": True})
```

### A Metaclasse is a callable
Weirdly enough, a metaclass can also be a simple callable as a function!

We can define custom metaclasses. It is just necessary to define a `__call__` method:
```
class MyMetaClass(type):
  def __call__(class_name, *args, **kwargs):
    print("I'm creating a class by means of MyMetaClass")
    return type(class_name, *args, **kwargs)
```
and this is used thusly
```
class MyClass(BaseClass, metaclass=MyMetaClass):
  pass
```

### `type` is an object itself
```
assert isinstance(type, object) == True
assert isinstance(object, type) == True
assert type(type) == type
assert type(object) == type
```
