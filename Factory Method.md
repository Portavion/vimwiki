# Factory Method

source: [Refactoring.guru](https://refactoring.guru/design-patterns/factory-method/python/example)

The factory method is a creational design pattern that provides an interface for creating objects in a superclass, but allows subclasses to alter the type of objects that will be created.
 
## Problem

Let's take the example of a logistic management application. The first version can only handle transportation by trucks so the bulk of the code lives inside the Truck class.

After a while, the app becomes popular and we decide to implement sea logistics into the app. 

Adding a new class to the program isn't simple as the rest of the code is tightly coupled to existing classes. It would required changes to the entire codebase. And if later we want to add a third method of transportation, this would also require changes again.

It creates messy code, riddles with conditionals that switch the app behavior depending on the class.

## Solution

The Factory Method pattern suggests that you replace direct object construction calls (using the new operator) with calls to a special factory method. Objects returned by a factory method are often referred to as products.

  * Factory: 
    * planDelivery() -> Transport t= createTransport() 
    * createTransport()
  * RoadLogistics: createTransport() -> return new Truck()
  * SeaLogistics: createTransport() -> return new Ship()

With this changes, we can now override the factory method in a subclass and change the class of products being created by the method.

The limitation is that subclasses may return different types of products only if these products have a common base class or interface. The factory method in the base class should have its return type declared as this interface. 

  * Interface Transport: deliver()
    * Truck: deliver() -- deliver by land in a box
    * Ship: deliver() -- deliver by sea in a container

Both Truck and Ship should implement the Transport interface, which declares a method called deliver. Each class implements this method differently: a truck delivers cargo by land, ships deliver cargo by sea. The factory method in the RoadLogistics class returns truck objects, whereas the factory method in the SeaLogistics class returns ships.

The code that uses the factory method (often called client code) doesn't see a difference between the actual products returned by the various subclasses. The client treats all the products as abstract Transport. The client knows that all transport objects are supposed to have the deliver method, but exactly how it works isn't important to the client.

## Structure

1. The Product declares the interface, which is common to all objects that can be produced by the creator and its subclasses
2. Concrete Products are different implementation of the product interface
3. The Creator class declares the factory method that returns new product objects. It's important that the return type of this method matches the product interface. You can declare the factory method as abstract to force all subclasses to implement their own versions of the method. As an alternative, the base factory method can return some default product type. Usually, the creator already have some core business logic related to products. The factory method helps to decouple this logic from the concrete product classes. 
4. Concrete Creators override the base factory method so it retusn a different type of product.

## Pseudocode

This example illustrates how the factory method can be used for creating cross-platform UI elements without coupling the client code to concrete UI classes.

```javascript
// The creator class declares the factory method that must
// return an object of a product class. The creator's subclasses
// usually provide the implementation of this method.
class Dialog is
    // The creator may also provide some default implementation
    // of the factory method.
    abstract method createButton():Button

    // Note that, despite its name, the creator's primary
    // responsibility isn't creating products. It usually
    // contains some core business logic that relies on product
    // objects returned by the factory method. Subclasses can
    // indirectly change that business logic by overriding the
    // factory method and returning a different type of product
    // from it.
    method render() is
        // Call the factory method to create a product object.
        Button okButton = createButton()
        // Now use the product.
        okButton.onClick(closeDialog)
        okButton.render()


// Concrete creators override the factory method to change the
// resulting product's type.
class WindowsDialog extends Dialog is
    method createButton():Button is
        return new WindowsButton()

class WebDialog extends Dialog is
    method createButton():Button is
        return new HTMLButton()


// The product interface declares the operations that all
// concrete products must implement.
interface Button is
    method render()
    method onClick(f)

// Concrete products provide various implementations of the
// product interface.
class WindowsButton implements Button is
    method render(a, b) is
        // Render a button in Windows style.
    method onClick(f) is
        // Bind a native OS click event.

class HTMLButton implements Button is
    method render(a, b) is
        // Return an HTML representation of a button.
    method onClick(f) is
        // Bind a web browser click event.


class Application is
    field dialog: Dialog

    // The application picks a creator's type depending on the
    // current configuration or environment settings.
    method initialize() is
        config = readApplicationConfigFile()

        if (config.OS == "Windows") then
            dialog = new WindowsDialog()
        else if (config.OS == "Web") then
            dialog = new WebDialog()
        else
            throw new Exception("Error! Unknown operating system.")

    // The client code works with an instance of a concrete
    // creator, albeit through its base interface. As long as
    // the client keeps working with the creator via the base
    // interface, you can pass it any creator's subclass.
    method main() is
        this.initialize()
        dialog.render()```

## Applications

### Use the factory method when you don't know beforhand the exact types and dependencies of the objects your code should work with.

The factory method separates product construction code from the code that actually uses the product. Making it easier to extend the product construction code independently from the rest of the code. To add a new product type, you just need to create a new creator subclass and override the factory method in it.

### Use the factory method when you want to provide users of your library or framework with a way to extend its internal components.

Inheritance is probably the easiest way to extend the default behavior of a library or framework. But how would the framework recognize that your subclass should be used instead of a standard component?

The solution is to reduce the code that constructs components across the framework into a single factory method and let anyone override this method in addition to extending the component itself.

Let’s see how that would work. Imagine that you write an app using an open source UI framework. Your app should have round buttons, but the framework only provides square ones. You extend the standard Button class with a glorious RoundButton subclass. But now you need to tell the main UIFramework class to use the new button subclass instead of a default one.

To achieve this, you create a subclass UIWithRoundButtons from a base framework class and override its createButton method. While this method returns Button objects in the base class, you make your subclass return RoundButton objects. Now use the UIWithRoundButtons class instead of UIFramework. And that’s about it!

### Use the factory method when you want to save system resources by reusing existing objects instead of rebuilding them each time

You often experience this need when dealing with large, resource-intensive objects such as database connections, file systems, and network resources.

Let’s think about what has to be done to reuse an existing object:

    First, you need to create some storage to keep track of all of the created objects.
    When someone requests an object, the program should look for a free object inside that pool.
    … and then return it to the client code.
    If there are no free objects, the program should create a new one (and add it to the pool).

That’s a lot of code! And it must all be put into a single place so that you don’t pollute the program with duplicate code.

Probably the most obvious and convenient place where this code could be placed is the constructor of the class whose objects we’re trying to reuse. However, a constructor must always return new objects by definition. It can’t return existing instances.

Therefore, you need to have a regular method capable of creating new objects as well as reusing existing ones. That sounds very much like a factory method.

## How to implement

1. Make all products follow the same interface. This interface should declare methods that make sense in every product.
2. Add an empty factory method inside the creator class. The return type of the method should match the common product interface
3. In the creator's code find all references to product constructors. One by on,e replace them with calls to the factory method, while extracting the product creation code into the factory method. You might need a temporary parameter to the factory method to control the type of returned product. The code of the factory method may look pretty ugly. It may have a large switch statement that picks which product class to instantiate. But we will fix it soon.
4. Now create a set of creator subclasses for each type of product listed in the factory method. Override the factory method in the subclasses and extract the appropriate bits of construction code form the base method.
5. If there are too many product types and it doesn't make sense to create subclasses for all of them, you can reuse the control parameter from the base class in subclasses.
6. After all the extractions, the base factory has become empty, you can make it abstract. If there is something left, you can make it a default behavior of the method.
## Pros and cons

### Pros

  * You avoid tight coupling between the creator and the concrete products.
  * Single Responsibility Principle. You can move the product creation code into one place in the program, making the code easier to support.
  * Open/Closed Principle. You can introduce new types of products into the program without breaking existing client code.

### Cons

 The code may become more complicated since you need to introduce a lot of new subclasses to implement the pattern. The best case scenario is when you’re introducing the pattern into an existing hierarchy of creator classes.

## Python Code

```python
from __future__ import annotations
from abc import ABC, abstractmethod

class Creator(ABC):
    """
    The Creator class declares the factory method that is supposed to return an
    object of a Product class. The Creator's subclasses usually provide the
    implementation of this method.
    """

    @abstractmethod
    def factory_method(self):
        """
        Note that the Creator may also provide some default implementation of
        the factory method.
        """
        pass

    def some_operation(self) -> str:
        """
        Also note that, despite its name, the Creator's primary responsibility
        is not creating products. Usually, it contains some core business logic
        that relies on Product objects, returned by the factory method.
        Subclasses can indirectly change that business logic by overriding the
        factory method and returning a different type of product from it.
        """

        # Call the factory method to create a Product object.
        product = self.factory_method()

        # Now, use the product.
        result = f"Creator: The same creator's code has just worked with {product.operation()}"

        return result


"""
Concrete Creators override the factory method in order to change the resulting
product's type.
"""


class ConcreteCreator1(Creator):
    """
    Note that the signature of the method still uses the abstract product type,
    even though the concrete product is actually returned from the method. This
    way the Creator can stay independent of concrete product classes.
    """

    def factory_method(self) -> Product:
        return ConcreteProduct1()


class ConcreteCreator2(Creator):
    def factory_method(self) -> Product:
        return ConcreteProduct2()


class Product(ABC):
    """
    The Product interface declares the operations that all concrete products
    must implement.
    """

    @abstractmethod
    def operation(self) -> str:
        pass


"""
Concrete Products provide various implementations of the Product interface.
"""


class ConcreteProduct1(Product):
    def operation(self) -> str:
        return "{Result of the ConcreteProduct1}"


class ConcreteProduct2(Product):
    def operation(self) -> str:
        return "{Result of the ConcreteProduct2}"


def client_code(creator: Creator) -> None:
    """
    The client code works with an instance of a concrete creator, albeit through
    its base interface. As long as the client keeps working with the creator via
    the base interface, you can pass it any creator's subclass.
    """

    print(f"Client: I'm not aware of the creator's class, but it still works.\n"
          f"{creator.some_operation()}", end="")


if __name__ == "__main__":
    print("App: Launched with the ConcreteCreator1.")
    client_code(ConcreteCreator1())
    print("\n")

    print("App: Launched with the ConcreteCreator2.")
    client_code(ConcreteCreator2())
```
