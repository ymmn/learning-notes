# Python Metaclasses

Everything in python is an object, including classes. Classes are objects of class `type`.

This means that you can make classes just like you can make objects:

```python
# class syntax
class Bar(Foo):
    i = 4
  
# equivalent
Bar = type('Bar', (Foo,), {i: 4})
```

It's basically `type(classname, parent_classes, class_attributes)`

### What is a Metaclass?
Well first, what's a regular class? A regular class is a class that creates objects.

In contrast, **a metaclass is a class the creates classes**. Since a class is an object of type `type`, a metaclass inherits from the `type` class.  This is tricky but makes sense.

```python
# a class that makes objects
class Foo(object):
    pass
  
# a class that makes animals
class Dog(Animal):
    pass
  
# a class that makes classes
class MyMetaClass(type):
    pass
```

### Metaclass Syntax
We can use a metaclass the same way we use a regular class, with the `type` constructor we specified above, even though it's kinda nasty:
```python
# ugh
MyClass = MyMetaClass('MyClass', parent_classes, class_attributes)
```

There's another syntax we can use for this:

```python
# python2.7 syntax
class MyClass(object):
    __metaclass__ = MyMetaClass
    myattr = 123
```

Doing this in python3 is different, so often you might use the `six` library:

```python
import six

@six.add_metaclass(MyMetaClass)
class MyClass(object):
    pass
```

### Adding functionality to a Metaclass

you add functionality in the metaclass's `__new__` method (or `__init__`, depending on what you want), which will run whenever the class is being defined:

```
class MyMetaClass(type):
    def __new__(cls, classname, parents, attributes):
        # open the specified file for writing
        if 'file' in dct:
            filename = dct['file']
            dct['file'] = open(filename, 'w')
        
        # we need to call type.__new__ to complete the initialization
        return super(InterfaceMeta, cls).__new__(cls, classname, parents, attributes)
```


### Resources
https://jakevdp.github.io/blog/2012/12/01/a-primer-on-python-metaclasses/
