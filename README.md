# Mutable vs Immutable Objects in Python

|Class|Description| Immutable |
|-----|-----------|-------|
|bool| Boolean value| Yes   |
|int | integer | Yes |
|float| floating-point number| Yes |
|tuple| immutable sequence of objects| Yes|
|str| character string | Yes |
|frozenset| immutable form of set class | Yes |
| set | unordered set of distinct objects | |
| list| mutable sequence of objects | |
| dict | assocoative mapping | |

# Who can be key of dict

Any type of object exclude list, dict and set.

# Decorators

Decorators are a very powerful and useful tool in Python since it allows programmers to modify the behaviour of a function or class. Decorators allow us to wrap another function in order to extend the behaviour of the wrapped function, without permanently modifying it. But before diving deep into decorators let us understand some concepts that will come in handy in learning the decorators.

```python
def uppercase_decorator(function):
    def wrapper(string):
        func = function(string)
        make_uppercase = func.upper()
        return make_uppercase

    return wrapper


@uppercase_decorator
def hello(string):
    return string


print(hello(string="Hello"))

```

### Output

HELLO

# Python Iterators

An iterator is an object that contains a countable number of values.
An iterator is an object that can be iterated upon, meaning that you can traverse through all the values.
Technically, in Python, an iterator is an object which implements the iterator protocol, which consist of the methods __iter__() and __next__().

## Create an Iterator

To create an object/class as an iterator you have to implement the methods __iter__() and __next__() to your object.
As you have learned in the Python Classes/Objects chapter, all classes have a function called __init__(), which allows you to do some initializing when the object is being created.
The __iter__() method acts similar, you can do operations (initializing etc.), but must always return the iterator object itself.
The __next__() method also allows you to do operations, and must return the next item in the sequence.

```python
class MyNumbers:
  def __iter__(self):
    self.a = 1
    return self

  def __next__(self):
    x = self.a
    self.a += 1
    return x

myclass = MyNumbers()
myiter = iter(myclass)

print(next(myiter))
print(next(myiter))
print(next(myiter))
print(next(myiter))
print(next(myiter)) 
```

# Python Generators

In Python, a generator is a function that returns an iterator that produces a sequence of values when iterated over.

Generators are useful when we want to produce a large sequence of values, but we don't want to store all of them in memory at once.

```python
def generator_name(arg):
    # statements
    yield arg
```

# Context manager

A context manager usually takes care of setting up some resource, e.g. opening a connection, and automatically handles the clean up when we are done with it.

```python
with open('/path/to/file.txt', 'r') as f:
  for line in f:
    print(line)
```

## How To Implement a Context Manager

There are two ways to implement a context manager. The first one is defining a class with implementations for the __enter__ and __exit__ methods. The second one is by creating a generator and using the contextlib.contextmanager decorator.

```python
class ToyExample:
    def __enter__(self):
        print("__enter__")
        return self
     
    def __exit__(self, exc_type, exc_val, exc_tb):
        print(f"__exit__({exc_type}, {exc_val}, {exc_tb}")

    def run(self):
        print("run")
```

# Python lambda



A lambda function is a small anonymous function.

A lambda function can take any number of arguments, but can only have one expression.
```python
x = lambda a : a + 10
print(x(5)) 
```

# GIL

The Python Global Interpreter Lock or GIL, in simple words, is a mutex (or a lock) that allows only one thread to hold the control of the Python interpreter.

This means that only one thread can be in a state of execution at any point in time. The impact of the GIL isn’t visible to developers who execute single-threaded programs, but it can be a performance bottleneck in CPU-bound and multi-threaded code.

Since the GIL allows only one thread to execute at a time even in a multi-threaded architecture with more than one CPU core, the GIL has gained a reputation as an “infamous” feature of Python.

Python uses reference counting for memory management. It means that objects created in Python have a reference count variable that keeps track of the number of references that point to the object. When this count reaches zero, the memory occupied by the object is released.

The problem was that this reference count variable needed protection from race conditions where two threads increase or decrease its value simultaneously. If this happens, it can cause either leaked memory that is never released or, even worse, incorrectly release the memory while a reference to that object still exists. This can cause crashes or other “weird” bugs in your Python programs.

But adding a lock to each object or groups of objects means multiple locks will exist which can cause another problem—Deadlocks (deadlocks can only happen if there is more than one lock). Another side effect would be decreased performance caused by the repeated acquisition and release of locks.

So, why was an approach that is seemingly so obstructing used in Python? Was it a bad decision by the developers of Python?

The GIL is simple to implement and was easily added to Python. It provides a performance increase to single-threaded programs as only one lock needs to be managed.

C libraries that were not thread-safe became easier to integrate. And these C extensions became one of the reasons why Python was readily adopted by different communities.

As you can see, the GIL was a pragmatic solution to a difficult problem that the CPython developers faced early on in Python’s life.