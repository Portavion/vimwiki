# Encapsulation

Encapsulation refers to the bundling of data (attributes) and methods that operates on the data into a single unit, typically a class.

Encapsulation also restricts direct access to some components, helping protect the integrity of the data and ensures proper usage.

Encapsulation is the process of hiding the internal state of an object and requiring all interactions to be performed through an object's methods. This :
  * Provides better control over data
  * Prevents accidental modification of data
  * Promotes modular programming

In python, this is achieved through public, protected and private attributes.

## How it works

### Data Hiding

The variables (attributes) are kept private or protected, they cannot be accessed directly outside the class. They have to be accessed or modified through the methods.

### Access through methods

Methods act as the interface through which external code interacts with the data stored in the variables. Getters and setters are common methods used to retrieve and update the value of a private variable.

### Control and security

The class can enforce rules on how the variables are accessed or modified, maintaining control and security over the data.

## Examples

Encapsulation is like having a bank account system where the balance (data) is kept private. You can't directly change it by accessing the bank's database. Instead, the bank provides methods like deposit and withdraw to modify your balance safely.
  * Private Data (Balance): your balance is stored securely. Direct access from outside is not allowed. 
  * Public methods (deposit and withdraw): these are the only way to modify your balance. They check the request follow the rules (e.g. you have enough funds) before allowing the changes.

## Public members

Public members are allowed both inside and outside the class. These are the default members in Python. 

## Protected members

Protected members are identified with a single underscore. They are meant to be accessed within the class or its subclasses. This is not enforced in Python. Protected are meant to be accessible but not modifiable.

```python
class Protected:
    def __init__(self):
        self._age = 30  # Protected attribute

class Subclass(Protected):
    def display_age(self):
```

## Private members

Private members are identified with a double underscore. They cannot be accessed outside of Python. This is enforced through name mangling. (renaming internally to make it hard to access). Private members are meant to not be accessible nor modifiable.

```python
class Private:
    def __init__(self):
        self.__salary = 50000  # Private attribute

    def get_salary(self):
        return self.__salary  # Access through public method
    def set_salary(self, new_salary):
        if not isinstance(new_salary, int):
            raise TypeError("Salary must be an integer")
        self.__salary = new_salary

obj = Private()
print(obj.get_salary())  # Works
obj.set_salary(25000) #works
#obj.__salary = 25000 #doesn't work
#print(obj.__salary)  # Raises AttributeError
```

## Decorators

These can be used to access a protected attribute and write cleaner code. The user cannot modify it.

```python
class Tree:
   def __init__(self, height):
       # First, create a private or protected attribute
       self.__height = height

   @property
   def height(self):
       return self.__height

pine = Tree(17)
pine.height
```

And if we want to add a setter:

```python
class Tree:
   def __init__(self, height):
       self.__height = height

   @property
   def height(self):
       return self.__height

   @height.setter
   def height(self, new_height):
       if not isinstance(new_height, int):
           raise TypeError("Tree height must be an integer")
       if 0 < new_height <= 40:
           self.__height = new_height
       else:
           raise ValueError("Invalid height for a pine tree")
      
pine = Tree(10)

pine.height = 33  # Calling @height.setter
pine.height = 45  # An error is raised

```
## Best practices

  1. Create protected or private attributes or methods if they are used only by you. They should not be included in the documentation to discourage others to use them.
  2. You don't always have to create properties for every single class attribute.
  3. Consider raising a warning every time a user accesses a protected member.
  4. Use private members sparingly as they can make code unreadable for those unfamiliar with the convention.
  5. Prioritise clarity over obscurity. Encapsulation aims to improve code reabability and maintainability, don't completely hide important implementation details of your class.
  6. If you want to create read-only properties, don't implement the @attr.setter method.
  7. Remember encapsulation is a convention, not an enforced aspect of Python.
  8. For simple classes, consider using dataclasses which allow class encapsulation with a single line of code. Dataclasses are for simpler classes with predictable attributes and methods.
