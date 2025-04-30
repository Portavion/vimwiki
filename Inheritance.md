# Inheritance

Inheritance is an OOP concept that allows a child class to inherit all the methods and properties from another class. 

  * Parent Class -- being inherited from, also called base class
  * Child class -- inherit from another class, also called derived class.

## Types of inheritance

  * Single inheritance: one parent class
  * Multiple inheritance: more than one parent class
  * Multilevel inheritance: derived from a class which is also derived from another class
  * Hierarchical inheritance: Multiple class inherit from a single parent class
  * Hybrid inheritance: combination of more than one type of inheritance

## Creating a Child class

You send the parent class as a parameter when creating the child class.

  * Use the pass keyword if not adding any other properties or methods to the class.

```python
class Student(Person):
  pass
```

### Init

When adding the __init__() function, the child no longer inherit the parent's __init__() fuction and you need to call it.

```python
class Student(Person):
  def __init__(self, fname, lname):
    Person.__init__(self, fname, lname)
```

### Super

Python has a super() function that will make the child class inherit all the methods and properties from its parent:

```python
class Student(Person):
  def __init__(self, fname, lname):
    super().__init__(self, fname, lname)
    self.graduationyear = 2019
```

### Methods

If you add a method in the child class with the same name as a function in the parent class, the inheritance of the parent method will be overidden.

```python
class Student(Person):
  def __init__(self, fname, lname):
    super().__init__(self, fname, lname)
    self.graduationyear = 2019
    
    def welcome(self):
      print("Welcome", self.firstname, self.lastname, "to the class of", self.graduationyear)

```
