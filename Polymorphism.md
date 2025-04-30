# Polymorphism

Polymorphism is an OOP concept that allows methods / functions / operators with the same name to be executed on many objects or classes.

## Function Polymorphism

Functions that can be executed on many objects or classes. For example:

  * len()

## Class Polymorphism

This is when we have multiple classes with the same methods name.

Example: 
* Car, Boat and Plane classes
* Each with the method move
* Can be called with Car.move(), Boat.move() etc...

Because of polymorphism we can execute the same method for multiple classes.

```python
class Car:
  def __init__(self, brand, model):
    self.brand = brand
    self.model = model

  def move(self):
    print("Drive!")

class Boat:
  def __init__(self, brand, model):
    self.brand = brand
    self.model = model

  def move(self):
    print("Sail!")

class Plane:
  def __init__(self, brand, model):
    self.brand = brand
    self.model = model

  def move(self):
    print("Fly!")

car1 = Car("Ford", "Mustang")       #Create a Car object
boat1 = Boat("Ibiza", "Touring 20") #Create a Boat object
plane1 = Plane("Boeing", "747")     #Create a Plane object

for x in (car1, boat1, plane1):
  x.move()
```

## Inheritance class polymorphism

We can use polymorphism to have child class and parent class with the same methods / properties name. 

In this case the child class method will override the parent class method or property.
