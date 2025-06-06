# Python init

init() method in Python is used to initialise objects of a class. 
It is also called a constructor. Constructors are used to initialize the object’s state.

The task of constructors is to initialize (assign values) to data members of the class when an object of the class is created. 
Like methods, a constructor also contains a collection of statements(i.e. instructions) that are executed at the time of Object creation. 
It is run as soon as an object of a class is instantiated.

All classes have a function called __init__(), which is always executed when the class is being initiated.

Use the __init__() function to assign values to object properties, or other operations that are necessary to do when the object is being created:

Create a class named Person, use the __init__() function to assign values for name and age:
```python
class Person:
  def __init__(self, name, age):
    self.name = name
    self.age = age

p1 = Person("John", 36)
```
