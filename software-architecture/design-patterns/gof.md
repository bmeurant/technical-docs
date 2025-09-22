---
title: GoF Design Patterns
tags:
  - gof
---

# GoF Design Patterns

**[[software-architecture/design-patterns/|Design patterns]]** are general, reusable solutions to common problems in software development. The book "Design Patterns: Elements of Reusable Object-Oriented Software" by the "Gang of Four" (**GoF**) — Erich Gamma, Richard Helm, Ralph Johnson, and John Vlissides — formalized 23 of these patterns, making them a cornerstone of object-oriented programming. These patterns are not ready-to-use code libraries but rather abstract descriptions of how objects and classes can interact to solve specific issues, improving code flexibility, reusability, and maintainability. They are grouped into three main categories: **Creational**, **Structural**, and **Behavioral**.

---

## 1. Creational Patterns

### **Singleton**

* **Problem**: How to ensure a class has only one instance and provide a global point of access to that instance?
* **Synopsis**: The **Singleton** pattern restricts the instantiation of a class to a single object, which is useful when exactly one object is needed to coordinate actions across the system. It is one of the most well-known but also often criticized patterns.

    ```mermaid
    classDiagram
        class Singleton {
            -static Singleton instance
            -Singleton()
            +static Singleton getInstance()
        }
    ```

    The class diagram shows the `Singleton` class with a private static attribute `instance`. The constructor is private (`-Singleton()`) to prevent direct instantiation from outside the class. The public static method `getInstance()` is the only way to obtain the unique instance, creating it if it doesn't already exist.

* **Key Characteristics**:
    * **Single Instance**: Guarantees only one instance of the class can be created.
    * **Global Access**: Provides a single, global access point to that instance.
    * **Lazy Initialization**: The instance is often created only when it is first needed.
* **Applicability**: Logger objects, configuration managers, or a single database connection pool.
* **Limitations and Challenges**: It can violate the **[[solid|Single Responsibility Principle]]** (SRP) and make testing difficult due to its global state. It's often considered an anti-pattern in modern development due to tight [[cohesion-coupling|coupling]].

### **Factory Method**

* **Problem**: How to define an interface for creating an object, but let subclasses decide which class to instantiate?
* **Synopsis**: The **Factory Method** pattern defers the instantiation of a class to its subclasses. It encapsulates the object creation logic, allowing the creation of different types of objects without changing the client code.

    ```mermaid
    classDiagram
        class Creator {
            &lt;&lt;abstract&gt;&gt;
            +createProduct() Product
            +someOperation()
        }
        class ConcreteCreator {
            +createProduct() ConcreteProduct
        }
        class Product {
            &lt;&lt;interface&gt;&gt;
            +operation()
        }
        class ConcreteProduct {
            +operation()
        }
        Creator <|-- ConcreteCreator
        Creator --> Product
        ConcreteCreator ..> ConcreteProduct : create
        Product <|.. ConcreteProduct
    ```

    The class diagram shows an abstract `Creator` class with an abstract `createProduct()` method. The `ConcreteCreator` subclass implements this method to instantiate a `ConcreteProduct`. The client code interacts with the abstract `Creator` and `Product` interfaces, decoupling it from the concrete implementations.

* **Key Characteristics**:
    * **[[cohesion-coupling|Decoupling]]**: Separates the client from the concrete products.
    * **Extensibility**: Easily add new products and creators without modifying existing code.
    * **Encapsulation**: The creation logic is encapsulated within the factory method.
* **Applicability**: When a class can't anticipate the class of objects it needs to create, or when subclasses must specify the objects they create.
* **Limitations and Challenges**: It can lead to an explosion of parallel class hierarchies if many different products are needed.

#### **Abstract Factory**

* **Problem**: How to provide an interface for creating families of related or dependent objects without specifying their concrete classes?
* **Synopsis**: The **Abstract Factory** pattern creates a "super-factory" that groups individual factories together without specifying their concrete classes. It provides a way to create a family of objects that belong together without specifying their concrete classes.

    ```mermaid
    classDiagram
        class Client {
            -factory:AbstractFactory
        }
        class AbstractFactory {
            &lt;&lt;interface&gt;&gt;
            +createProductA() ProductA
            +createProductB() ProductB
        }
        class ConcreteFactory1 {
            +createProductA() ProductA
            +createProductB() ProductB
        }
        class ConcreteFactory2 {
            +createProductA() ProductA
            +createProductB() ProductB
        }
        class ProductA {
            &lt;&lt;interface&gt;&gt;
        }
        class ConcreteProductA1
        class ConcreteProductA2

        class ProductB {
            &lt;&lt;interface&gt;&gt;
        }
        class ConcreteProductB1
        class ConcreteProductB2

        Client --> AbstractFactory
        AbstractFactory <|.. ConcreteFactory1
        AbstractFactory <|.. ConcreteFactory2
        ProductA <|.. ConcreteProductA1
        ProductA <|.. ConcreteProductA2
        ProductB <|.. ConcreteProductB1
        ProductB <|.. ConcreteProductB2
        ConcreteFactory1 ..> ConcreteProductA1 : create
        ConcreteFactory1 ..> ConcreteProductB1 : create
        ConcreteFactory2 ..> ConcreteProductA2 : create
        ConcreteFactory2 ..> ConcreteProductB2 : create
    ```

    The diagram shows an `AbstractFactory` interface that defines methods for creating a family of related products. The `ConcreteFactory` classes implement this interface to create a specific family of products. This ensures that a client using a `ConcreteFactory1` will always receive objects from the same family (`ConcreteProductA1` and `ConcreteProductB1`).

* **Key Characteristics**:
    * **Family of Products**: Creates families of objects that are designed to work together.
    * **[[cohesion-coupling|Loose Coupling]]**: Decouples the client code from the concrete implementations of the products.
    * **Consistency**: Ensures that the client uses objects from the same family.
* **Applicability**: When a system must be independent of how its products are created, or when a family of related product objects needs to be created.
* **Limitations and Challenges**: It can be overly complex for a small number of products. Adding new types of products to the family requires changing the `AbstractFactory` interface and all concrete factories.

#### **Builder**

* **Problem**: How to separate the construction of a complex object from its representation, so that the same construction process can create different representations?
* **Synopsis**: The **Builder** pattern provides a step-by-step approach to creating a complex object. It allows you to build different variations of an object by using the same construction process.

    ```mermaid
    classDiagram
        class Client
        class Director {
            -builder:Builder
            +construct()
        }
        class Builder {
            &lt;&lt;interface&gt;&gt;
            +buildPartA()
            +buildPartB()
            +getResult()
        }
        class ConcreteBuilder {
            +buildPartA()
            +buildPartB()
            +getResult()
        }
        class Product
        Director --> Builder
        Builder <|.. ConcreteBuilder
        ConcreteBuilder ..> Product : create
        Client --> Director
        Client ..> ConcreteBuilder
    ```

    The diagram shows a `Director` that orchestrates the construction process by calling methods on a `Builder` interface. A `ConcreteBuilder` implements the `Builder` interface to construct a `Product` in a specific way. The `Director`'s role is to ensure the correct sequence of steps, while the `Builder`'s role is to implement the logic for each step.

* **Key Characteristics**:
    * **Step-by-step Construction**: Builds complex objects incrementally.
    * **Flexibility**: The same construction process can create different representations of an object.
    * **Encapsulation**: The internal representation of the product is hidden from the client.
* **Applicability**: When a complex object's construction should be independent of the parts that make up the object, or when the creation process needs to be customized.
* **Limitations and Challenges**: Can introduce more classes than necessary for a simple object. The `Director` can become a central point of control, potentially leading to a **God Object**.

#### **Prototype**

* **Problem**: How to specify the kinds of objects to create by using a prototypical instance and creating new objects by copying this prototype?
* **Synopsis**: The **Prototype** pattern creates new objects by cloning an existing object, which can be more efficient than creating a new object from scratch, especially for complex objects.

    ```mermaid
    classDiagram
    class Client
    class Prototype {
        &lt;&lt;interface&gt;&gt;
        +clone() Prototype
    }
    class ConcretePrototype1 {
        -field1
        +clone() Prototype
    }
    class ConcretePrototype2 {
        -field2
        +clone() Prototype
    }
    Prototype <|.. ConcretePrototype1
    Prototype <|.. ConcretePrototype2
    Client --> Prototype
    ```

    The diagram shows a `Prototype` interface with a `clone()` method. The `ConcretePrototype` classes implement this method to create copies of themselves. The client simply calls `clone()` on a prototype instance to get a new object, without knowing its concrete class.

* **Key Characteristics**:
    * **Cloning**: Creates new objects by copying an existing one.
    * **Runtime Customization**: New object types can be added at runtime by providing a new prototype.
    * **No Class-Specific Code**: The client can create new objects without being [[cohesion-coupling|coupled]] to their concrete classes.
* **Applicability**: When a system should be independent of how its objects are created, composed, and represented, or when an object is costly to create.
* **Limitations and Challenges**: Managing **shallow copy** vs. **deep copy** can be complex. Objects with circular references or complex internal structures can be challenging to clone correctly.

---

### 2. Structural Patterns

#### **Adapter**

* **Problem**: How to allow classes with incompatible interfaces to work together?
* **Synopsis**: The **Adapter** pattern acts as a bridge between two incompatible interfaces. It converts the interface of a class into another interface that clients expect, allowing for collaboration.

    ```mermaid
    classDiagram
        class Client
        class Target {
            &lt;&lt;interface&gt;&gt;
            +request()
        }
        class Adaptee {
            +specificRequest()
        }
        class Adapter {
            +request()
        }
        Client --> Target
        Adapter ..|> Target
        Adapter o-- Adaptee
    ```

    The diagram shows a `Client` that needs to work with a `Target` interface. The `Adaptee` is an existing class with a different interface. The `Adapter` class implements the `Target` interface, but internally it delegates the call to the `Adaptee`'s incompatible method.

* **Key Characteristics**:
    * **Interface Conversion**: Converts one interface to another.
    * **[[cohesion-coupling|Decoupling]]**: Decouples the client from the `Adaptee`'s specific interface.
    * **Encapsulation**: Hides the incompatibility from the client.
* **Applicability**: Integrating third-party libraries, modernizing a legacy system, or reusing classes that have incompatible interfaces.
* **Limitations and Challenges**: Can become complex if the adaptation logic is extensive. A complex adapter may violate the **[[solid|Single Responsibility Principle]]** (SRP).

#### **Bridge**

* **Problem**: How to decouple an abstraction from its implementation so that the two can vary independently?
* **Synopsis**: The **Bridge** pattern separates a class into two distinct hierarchies: one for the abstraction and one for the implementation. This allows both hierarchies to evolve independently, promoting flexibility and extensibility.

    ```mermaid
    classDiagram
        class Client
        class Abstraction {
            -implementation : Implementation
            +operation()
        }
        class RefinedAbstraction {
            +operation()
        }
        class Implementation {
            &lt;&lt;interface&gt;&gt;
            +operationImpl()
        }
        class ConcreteImplementationA {
            +operationImpl()
        }
        class ConcreteImplementationB {
            +operationImpl()
        }
        Client --> Abstraction
        Abstraction o-- Implementation
        Abstraction <|-- RefinedAbstraction
        Implementation <|.. ConcreteImplementationA
        Implementation <|.. ConcreteImplementationB
    ```

    The class diagram shows an `Abstraction` class that holds a reference to an `Implementation`. The `Abstraction` and `Implementation` are the two independent hierarchies. `RefinedAbstraction` extends the `Abstraction` hierarchy, while `ConcreteImplementation` classes implement the `Implementation` interface. The `Abstraction` acts as a **bridge**, delegating its `operation()` call to the `Implementation`'s `operationImpl()`.

* **Key Characteristics**:
    * **[[cohesion-coupling|Decoupling]]**: Separates the abstraction from its implementation.
    * **Independent Variation**: Allows both class hierarchies to be extended independently.
    * **Composition over Inheritance**: Uses composition to link the abstraction and implementation.
* **Applicability**: When a system needs to be extensible in two independent dimensions, such as different platforms (Windows, macOS) and different features (shapes, colors).
* **Limitations and Challenges**: Adds complexity to the design, as it introduces two new layers of abstraction. It's often overkill for a simple problem.

#### **Composite**

* **Problem**: How to compose objects into tree structures to represent part-whole hierarchies, allowing clients to treat individual objects and compositions of objects uniformly?
* **Synopsis**: The **Composite** pattern organizes objects into a tree structure to represent [[posa|part-whole hierarchies]]. It allows clients to interact with individual objects or groups of objects in a consistent manner.

    ```mermaid
    classDiagram
        class Client
        class Component {
            &lt;&lt;interface&gt;&gt;
            +operation()
            +add(Component)
            +remove(Component)
        }
        class Leaf {
            +operation()
            +add(Component)
            +remove(Component)
        }
        class Composite {
            +operation()
            +add(Component)
            +remove(Component)
        }
        Component <|.. Leaf
        Component <|.. Composite
        Composite o-- "many" Component
        Client --> Component
    ```

    The diagram shows a common `Component` interface that is implemented by both `Leaf` objects and `Composite` objects. The `Composite` object holds a collection of other `Component` objects (both `Leaf` and `Composite`), forming a recursive structure. This allows a client to call `operation()` on any `Component` object without knowing if it's an individual object or a group.

* **Key Characteristics**:
    * **Tree Structure**: Organizes objects in a tree-like hierarchy.
    * **Uniformity**: The client can treat individual objects and compositions uniformly.
    * **Recursion**: Operations can be performed recursively on the entire hierarchy.
* **Applicability**: Representing part-whole hierarchies such as file systems, graphical user interface elements, or an organizational chart.
* **Limitations and Challenges**: The `Composite` can introduce unnecessary complexity for a simple, flat structure. It can also be challenging to restrict the types of components that can be added to a composite.

#### **Decorator**

* **Problem**: How to attach additional responsibilities to an object dynamically and transparently, without affecting other objects?
* **Synopsis**: The **Decorator** pattern wraps an object with another object that adds new functionality. It provides a flexible alternative to subclassing for extending functionality.

    ```mermaid
    classDiagram
        class Component {
            &lt;&lt;interface&gt;&gt;
            +operation()
        }
        class ConcreteComponent {
            +operation()
        }
        class Decorator {
            &lt;&lt;abstract&gt;&gt;
            -component : Component
            +operation()
        }
        class ConcreteDecoratorA {
            +operation()
            -addedState
        }
        class ConcreteDecoratorB {
            +operation()
            -addedBehavior()
        }
        Component <|.. ConcreteComponent
        Component <|.. Decorator
        Decorator "1" o-- "1" Component : wraps
        Decorator <|-- ConcreteDecoratorA
        Decorator <|-- ConcreteDecoratorB
    ```

    The class diagram shows a base `Component` interface and a `ConcreteComponent`. The `Decorator` is an abstract class that also implements the `Component` interface and holds a reference to a `Component`. `ConcreteDecorator` objects extend the `Decorator` and add new functionality by wrapping a component.

* **Key Characteristics**:
    * **Dynamic Responsibility**: Adds responsibilities at runtime.
    * **Composition**: Uses composition instead of inheritance.
    * **[[solid|Open/Closed Principle]]**: You can add new functionality without modifying existing code.
* **Applicability**: Adding features to a data stream (compression, encryption), adding menus and scrollbars to a window, or adding behaviors to a game object.
* **Limitations and Challenges**: Can lead to a large number of small, tightly coupled objects. Debugging can be complex when an object is wrapped in a long chain of decorators.

#### **Facade**

* **Problem**: How to provide a simplified interface to a complex set of subsystems?
* **Synopsis**: The **Facade** pattern provides a single, unified interface to a complex system. It simplifies the client's interaction with the system by hiding its complexity and managing the interactions between its subsystems.

    ```mermaid
    classDiagram
        class Client
        class Facade {
            +operation()
        }
        class SubsystemA {
            +operationA()
        }
        class SubsystemB {
            +operationB()
        }
        class SubsystemC {
            +operationC()
        }
        Client --> Facade
        Facade --> SubsystemA
        Facade --> SubsystemB
        Facade --> SubsystemC
    ```

    The diagram shows the `Facade` class acting as the sole point of entry for the `Client`. The `Facade` manages the calls to the various subsystems (`SubsystemA`, `SubsystemB`, `SubsystemC`), which are not directly accessible to the client. This reduces the client's coupling to the subsystems and simplifies the API.

* **Key Characteristics**:
    * **Simplification**: Provides a simplified, high-level interface.
    * **[[cohesion-coupling|Loose Coupling]]**: Decouples the client from the complex subsystem.
    * **Layering**: Defines a new layer that separates the client from the internal structure.
* **Applicability**: Simplifying the API of a complex library, creating a high-level interface for a legacy system, or structuring a complex application into [[layered|layers]].
* **Limitations and Challenges**: The `Facade` can become a **God Object** if it accumulates too many responsibilities. It may also hide some of the subsystem's useful functionality from the client.

#### **Flyweight**

* **Problem**: How to support a large number of fine-grained objects efficiently by using sharing?
* **Synopsis**: The **Flyweight** pattern minimizes memory usage by sharing state between multiple objects. It achieves this by separating the object's intrinsic (shared) state from its extrinsic (unique) state.

    ```mermaid
    classDiagram
        class Client
        class FlyweightFactory {
            -flyweights
            +getFlyweight(key)
        }
        class Flyweight {
            &lt;&lt;interface&gt;&gt;
            +operation(extrinsicState)
        }
        class ConcreteFlyweight {
            -intrinsicState
            +operation(extrinsicState)
        }
        class UnsharedConcreteFlyweight {
            -allState
            +operation()
        }
        Client --> FlyweightFactory
        FlyweightFactory o-- Flyweight
        Flyweight <|-- ConcreteFlyweight
        Flyweight <|-- UnsharedConcreteFlyweight
    ```

    The diagram shows a `FlyweightFactory` that manages a pool of shared `Flyweight` objects. The `Flyweight` interface defines a method that takes the extrinsic state as a parameter. `ConcreteFlyweight` stores the intrinsic state, while the client is responsible for passing the extrinsic state.

* **Key Characteristics**:
    * **Memory Optimization**: Reduces memory consumption by sharing objects.
    * **State Separation**: Clearly separates the shared intrinsic state from the unshared extrinsic state.
    * **Immutability**: The intrinsic state of a `Flyweight` object is often immutable.
* **Applicability**: In systems that need to instantiate a large number of similar objects, such as text editors (sharing character glyphs) or graphical applications.
* **Limitations and Challenges**: The pattern can be complex to implement correctly. It requires a clear distinction between the intrinsic and extrinsic state, which is not always straightforward.

#### **Proxy**

* **Problem**: How to provide a substitute or placeholder for another object to control access to it?
* **Synopsis**: The **[[posa|Proxy]]** pattern acts as an intermediary for a real object. It can add functionality such as lazy initialization, access control, or logging without modifying the original object.

    ```mermaid
    classDiagram
        class Client
        class Subject {
            &lt;&lt;interface&gt;&gt;
            +request()
        }
        class RealSubject {
            +request()
        }
        class Proxy {
            -realSubject
            +checkAccess()
            +request()
        }
        Client --> Subject
        Proxy ..|> Subject
        RealSubject ..|> Subject
        Proxy o-- RealSubject
    ```

    The diagram shows the `Client` interacting with the `Proxy` and the `RealSubject` via a common `Subject` interface. The `Proxy` holds a reference to the `RealSubject` and forwards the method calls to it, adding its own logic before or after the call.

* **Key Characteristics**:
    * **Control Access**: Controls access to the real object.
    * **Transparency**: The client interacts with the `Proxy` in the same way as with the real object.
    * **Functionality Addition**: Can add functionalities like lazy loading, caching, or security.
* **Applicability**: **Lazy Loading** (delaying resource-intensive object creation), **Protection Proxy** (checking access permissions), **Remote Proxy** (managing communication with a remote object), or **Logging Proxy** (logging method calls).
* **Limitations and Challenges**: Can introduce a performance overhead due to the added layer of indirection. It can also lead to a complex class structure if a proxy is used for every object.

---

### 3. Behavioral Patterns

#### **Chain of Responsibility**

* **Problem**: How to avoid coupling the sender of a request to its receiver by giving multiple objects a chance to handle the request?
* **Synopsis**: The **Chain of Responsibility** pattern creates a chain of objects. A request is passed along the chain until an object handles it. Each object in the chain has a reference to its successor.

    ```mermaid
    classDiagram
        class Client
        class Handler {
            &lt;&lt;interface&gt;&gt;
            +handle(request)
            +setNext(handler)
        }
        class AbstractHandler {
            &lt;&lt;abstract&gt;&gt;
            -next
            +handle(request)
            +setNext(handler)
        }
        class ConcreteHandler {
            +handle(request)
        }
        Client --> Handler
        AbstractHandler ..|> Handler
        AbstractHandler o--> Handler : successor
        AbstractHandler <|-- ConcreteHandler
    ```

    The class diagram shows a `Handler` interface that defines the request-handling method and the method to set the next handler in the chain. The `ConcreteHandler` classes implement the handling logic. If a request cannot be handled by a specific handler, it is passed to the next handler in the chain.

* **Key Characteristics**:
    * **[[cohesion-coupling|Loose Coupling]]**: Decouples the sender from the receiver.
    * **Flexibility**: The handlers and their order can be changed at runtime.
    * **Request Processing**: Allows multiple objects to handle a request without explicit conditional logic.
* **Applicability**: Event handling systems, validation pipelines, or request filtering in web servers.
* **Limitations and Challenges**: There is no guarantee that the request will be handled. Debugging can be complex if the chain is long and the flow is difficult to follow.

#### **Command**

* **Problem**: How to encapsulate a request as an object, thereby decoupling the object that issues the request from the object that performs the action?
* **Synopsis**: The **[[posa|Command]]** pattern turns a request into a stand-alone object containing all the information about the request. This allows for parameterizing clients with different requests, queuing requests, and supporting undoable operations.

    ```mermaid
    classDiagram
        class Client
        class Invoker {
            +setCommand(Command command)
            +executeCommand()
        }
        class Command {
            &lt;&lt;interface&gt;&gt;
            +execute()
        }
        class ConcreteCommand {
            -receiver : Receiver
            +execute()
        }
        class Receiver {
            +action()
        }
        Client --> Invoker
        Client --> ConcreteCommand
        Invoker --> Command
        ConcreteCommand --> Receiver
        ConcreteCommand ..|> Command
    ```
    The class diagram shows a `Command` interface with a single `execute()` method. A `ConcreteCommand` implements this interface and holds a reference to a `Receiver` (the object that knows how to perform the action). The `Invoker` holds a reference to a `Command` object and simply calls its `execute()` method, without knowing the details of the concrete operation or the receiver. The `Client` creates the `ConcreteCommand` and configures it with the appropriate `Receiver` before passing it to the `Invoker`.

* **Key Characteristics**:
    * **Command**: An interface for an operation.
    * **Concrete Command**: Implements the `Command` interface and holds a reference to a `Receiver`.
    * **Invoker**: The object that requests a `Command` to be executed. It doesn't know the concrete `Command` class.
    * **Receiver**: The object that performs the actual action.
* **Applicability**: Implementing undo/redo functionality, queuing tasks, or creating macro capabilities.
* **Limitations and Challenges**: Can result in a large number of `Command` classes if there are many different requests.

#### **Iterator**

* **Problem**: How to provide a way to access the elements of an aggregate object sequentially without exposing its underlying representation?
* **Synopsis**: The **Iterator** pattern abstracts the traversal of a collection. It provides a consistent interface to iterate over different types of collections without the client needing to know their internal structure.

    ```mermaid
    classDiagram
        class Client
        class Aggregate {
            &lt;&lt;interface&gt;&gt;
            +createIterator() Iterator
        }
        class ConcreteAggregate {
            +createIterator() ConcreteIterator
        }
        class Iterator {
            &lt;&lt;interface&gt;&gt;
            +hasNext() bool
            +next() Object
        }
        class ConcreteIterator {
            +hasNext() bool
            +next() Object
        }
        Aggregate <|.. ConcreteAggregate
        Iterator <|.. ConcreteIterator
        ConcreteAggregate o-- ConcreteIterator
        Client --> Iterator
        Client --> Aggregate
    ```

    The class diagram shows an `Aggregate` interface that defines a `createIterator()` method. The `ConcreteAggregate` implements this method to create a `ConcreteIterator`. The `Iterator` interface provides a common set of methods for traversing a collection (`hasNext()`, `next()`), which are implemented by the `ConcreteIterator`.

* **Key Characteristics**:
    * **Encapsulation**: Hides the internal structure of the collection.
    * **Multiple Traversals**: Allows multiple concurrent traversals over the same collection.
    * **[[solid|Single Responsibility Principle]]**: Separates the traversal logic from the collection itself.
* **Applicability**: Traversing a variety of data structures such as lists, trees, or graphs without exposing their internal implementation.
* **Limitations and Challenges**: For simple collections, it may introduce a small amount of unnecessary overhead.

#### **Mediator**

* **Problem**: How to simplify communication between multiple objects by centralizing it through a single object, thereby reducing a complex web of direct dependencies?
* **Synopsis**: The **Mediator** pattern provides a central object that coordinates the interactions between a group of objects. It promotes [[cohesion-coupling|loose coupling]] by ensuring that objects do not need to know about each other explicitly.

    ```mermaid
    classDiagram
        class Mediator {
            &lt;&lt;interface&gt;&gt;
            +send(message, colleague)
        }
        class ConcreteMediator {
            -colleagues
            +send(message, colleague)
        }
        class Colleague {
            &lt;&lt;abstract&gt;&gt;
            -mediator
            +send(message)
            +receive(message)
        }
        class ConcreteColleagueA
        class ConcreteColleagueB
        Mediator <|.. ConcreteMediator
        ConcreteMediator "1" o-- "*" Colleague : manages
        Colleague <|-- ConcreteColleagueA
        Colleague <|-- ConcreteColleagueB
    ```

    The diagram shows a `Mediator` interface and a `ConcreteMediator` that manages a group of `Colleague` objects. The `Colleague` objects do not communicate directly with each other. Instead, they send messages to the `Mediator`, which then forwards them to the appropriate `Colleague` or `Colleagues`.

* **Key Characteristics**:
    * **[[cohesion-coupling|Loose Coupling]]**: Objects are not directly coupled to each other.
    * **Centralized Control**: All communication logic is centralized in the `Mediator`.
    * **Reduced Subclassing**: Behavior can be changed by using a different `Mediator` class.
* **Applicability**: When objects interact in complex ways, such as in dialogue box systems, chat applications, or air traffic control systems.
* **Limitations and Challenges**: The `Mediator` can become a **God Object** if the system's logic is heavily centralized. This can make the mediator hard to maintain and modify.

#### **Memento**

* **Problem**: How to capture and externalize an object's internal state without violating encapsulation, so that the object can be restored to this state later?
* **Synopsis**: The **Memento** pattern provides a way to save and restore an object's state. It involves a `Caretaker` that stores a `Memento` object, which holds a snapshot of an `Originator`'s state. The `Caretaker` cannot access the `Memento`'s content, preserving encapsulation.

    ```mermaid
    classDiagram
        class Originator {
            +createMemento() Memento
            +restore(Memento)
            -state
        }
        class Memento {
            -state
            +getState()
        }
        class Caretaker {
            -memento
        }
        Originator --> Memento : create
        Caretaker o--> Memento : store
    ```

    The diagram shows the `Originator` object creating a `Memento` with its current state. The `Caretaker` receives this `Memento` and stores it. Later, the `Caretaker` can pass the `Memento` back to the `Originator` to restore its state. The `Caretaker` cannot access the `Memento`'s internal state, upholding encapsulation.

* **Key Characteristics**:
    * **State Saving**: Allows for saving an object's state.
    * **Encapsulation**: The internal state of the `Memento` is not exposed to the outside world.
    * **Undo/Redo**: Enables functionality like undoing and redoing operations.
* **Applicability**: Implementing undo/redo features, creating save points in a game, or any system requiring a history of object states.
* **Limitations and Challenges**: The `Memento` object can be very large if the originator's state is big, potentially consuming significant memory.

#### **Observer**

* **Problem**: How to define a one-to-many dependency between objects so that when one object changes state, all its dependents are notified and updated automatically?
* **Synopsis**: The **Observer** pattern defines a **Subject** (the one being observed) that maintains a list of **Observers** (the ones watching). When the state of the `Subject` changes, it notifies all its `Observers`, which then update themselves.

    ```mermaid
    classDiagram
        class Subject {
            +attach(Observer)
            +detach(Observer)
            +notify()
        }
        class Observer {
            &lt;&lt;interface&gt;&gt;
            +update()
        }
        class ConcreteObserver {
            +update()
        }
        Subject "1" o-- "*" Observer : manages
        ConcreteObserver ..|> Observer
    ```

    The class diagram shows a `Subject` that has methods to attach, detach, and notify observers. An `Observer` is an interface with an `update()` method. `ConcreteObserver` objects implement the `Observer` interface and are registered with a `Subject` to be notified of state changes.

* **Key Characteristics**:
    * **[[cohesion-coupling|Loose Coupling]]**: The subject and observers are loosely coupled.
    * **Flexibility**: New observers can be added without modifying the subject.
    * **Broadcast Communication**: The subject can broadcast notifications to all observers.
* **Applicability**: **[[event-driven|Event-driven]]** architectures, UI frameworks, **[[publish-subscribe|Pub/Sub systems]]**, or any system where a change in one object requires other objects to be updated.
* **Limitations and Challenges**: The order of notification is not guaranteed. If there are many observers, the notification process can be slow. It can also introduce **memory leaks** if observers are not properly detached.

#### **State**

* **Problem**: How to allow an object to alter its behavior when its internal state changes, making it appear as if the object has changed its class?
* **Synopsis**: The **State** pattern represents the different states of an object as separate classes. The object's behavior is then delegated to the current state object, which can transition the object to a new state.

    ```mermaid
    classDiagram
        class Context {
            -state
            +request()
        }
        class State {
            &lt;&lt;interface&gt;&gt;
            +handle()
        }
        class ConcreteStateA {
            +handle()
        }
        class ConcreteStateB {
            +handle()
        }
        Context "1" o-- "1" State
        State <|.. ConcreteStateA
        State <|.. ConcreteStateB
    ```

    The diagram shows a `Context` that holds a reference to a `State` object. The `Context`'s behavior is defined by the `State` object it currently holds. The `ConcreteState` classes implement the `State` interface and can transition the `Context` to a new state by changing its internal `State` object.

* **Key Characteristics**:
    * **Behavioral Partitioning**: Behavior is partitioned into different state classes.
    * **Encapsulation**: State-specific behavior is encapsulated within state objects.
    * **Eliminates Conditionals**: It replaces large conditional statements with polymorphic behavior.
* **Applicability**: Implementing state machines, modeling the lifecycle of an object (e.g., an `Order`'s status), or creating a simple workflow engine.
* **Limitations and Challenges**: Can lead to an explosion in the number of classes if there are many states. For a simple state machine, a simple conditional (`if`/`else`) statement might be sufficient.

#### **Strategy**

* **Problem**: How to define a family of algorithms, encapsulate each one, and make them interchangeable?
* **Synopsis**: The **Strategy** pattern encapsulates different algorithms within separate classes. It allows a client to select and use different algorithms at runtime, without having to change the client's code.

    ```mermaid
    classDiagram
        class Context {
            +strategy : Strategy
            +executeStrategy()
        }
        class Strategy {
            &lt;&lt;interface&gt;&gt;
            +execute()
        }
        class ConcreteStrategyA {
            +execute()
        }
        class ConcreteStrategyB {
            +execute()
        }
        Context "1" o--> "1" Strategy : has
        Strategy <|.. ConcreteStrategyA
        Strategy <|.. ConcreteStrategyB
    ```

    The diagram shows a `Context` that has a reference to a `Strategy` object. The `Strategy` interface defines a common method, `execute()`, that is implemented by `ConcreteStrategy` classes. The `Context` uses the `Strategy` to perform an action, allowing the algorithm to be swapped out at runtime.

* **Key Characteristics**:
    * **Algorithmic Encapsulation**: Each algorithm is a separate class.
    * **Interchangeable Behavior**: Allows for swapping algorithms at runtime.
    * **[[cohesion-coupling|Loose Coupling]]**: The client is decoupled from the specific algorithm implementation.
* **Applicability**: Sorting algorithms, pricing calculation systems, or different payment methods in an e-commerce application.
* **Limitations and Challenges**: Can lead to an increase in the number of classes. The client must be aware of the different strategies and choose the correct one.

#### **Template Method**

* **Problem**: How to define the skeleton of an algorithm in an operation, deferring some steps to subclasses without changing the algorithm's structure?
* **Synopsis**: The **Template Method** pattern defines a method that contains a fixed sequence of steps. Some of these steps are abstract and must be implemented by subclasses, allowing them to customize the behavior while following the main algorithm's structure.

    ```mermaid
    classDiagram
        class AbstractClass {
            &lt;&lt;abstract&gt;&gt;
            +templateMethod()
            +operation1()
            +operation2()
        }
        class ConcreteClass {
            +operation1()
            +operation2()
        }
        AbstractClass <|-- ConcreteClass
    ```

    The diagram shows an `AbstractClass` with a `templateMethod()`. This method calls other abstract primitive operations. The `ConcreteClass` extends the `AbstractClass` and provides concrete implementations for the primitive operations, thereby customizing the algorithm's behavior.

* **Key Characteristics**:
    * **Algorithmic Skeleton**: Defines a fixed sequence of steps.
    * **Behavioral Customization**: Subclasses can customize specific steps.
    * **[[solid|Inversion of Control]]**: The base class controls the overall algorithm, while the subclasses provide the details.
* **Applicability**: Frameworks, algorithms with a fixed structure, or any situation where different subclasses need to follow a common process but with variations in specific steps.
* **Limitations and Challenges**: The rigid structure of the template can be a limitation if the algorithm needs to be changed significantly in the future.

#### **Visitor**

* **Problem**: How to represent an operation to be performed on the elements of an object structure, allowing you to define a new operation without changing the classes of the elements on which it operates?
* **Synopsis**: The **Visitor** pattern separates an algorithm from the object structure on which it operates. This allows adding new operations to a class hierarchy without modifying the classes themselves.

    ```mermaid
    classDiagram
        class Client
        class Element {
            lt;&lt;interface&gt;&gt;
            +accept(Visitor)
        }
        class ElementA {
            +accept(Visitor)
        }
        class ElementB {
            +accept(Visitor)
        }
        class Visitor {
            lt;&lt;interface&gt;&gt;
            +visit(ElementA)
            +visit(ElementB)
        }
        class ConcreteVisitor
        Element <|.. ElementA
        Element <|.. ElementB
        ElementA <.. Visitor : calls
        ElementB <.. Visitor : calls
        Visitor <|.. ConcreteVisitor
        Client ..> Element
        Client ..> ConcreteVisitor
    ```

    The diagram shows a set of `Element` classes that have an `accept(Visitor)` method. A separate `Visitor` interface defines a `visit()` method for each type of element. A `ConcreteVisitor` implements these visit methods to perform a specific operation. This allows adding new operations (new `ConcreteVisitor` classes) without modifying the element classes.

* **Key Characteristics**:
    * **Separation of Concerns**: Separates the algorithm from the object structure.
    * **Extensibility**: Allows for adding new operations easily.
    * **Centralized Logic**: All logic for a specific operation is centralized in a single `Visitor` class.
* **Applicability**: Compilers (visiting a syntax tree), exporting data to different formats (XML, JSON), or performing operations on a complex object structure.
* **Limitations and Challenges**: Adding a new element type requires modifying all existing `Visitor` classes.

---

## **Resources & links**

### **Articles**

1.  **[Design Patterns](https://refactoring.guru/design-patterns)**

    This **Refactoring.Guru** article explains **design patterns** as proven solutions to common software design problems. It catalogues 22 classic patterns, discusses their benefits and classification (creational, structural, behavioral), and addresses the history and criticisms of these patterns.

2.  **[Design Patterns](https://en.wikipedia.org/wiki/Design_Patterns)**

    This **Wikipedia** article describes the influential book "Design Patterns: Elements of Reusable Object-Oriented Software" by the "Gang of Four" (GoF), which presents 23 software design patterns. It highlights key concepts like programming to an interface and object composition, and categorizes patterns into creational, structural, and behavioral types.

---

### **Videos**

1.  **[Appendix: Introduction to the Case Study of Gang of Four Patterns](https://www.youtube.com/watch?v=aJn2sJBNxv8)**

    This video by **Douglas Schmidt** is an introduction to a case study on **Gang of Four (GoF) patterns**, part of a larger series. It describes an object-oriented expression tree processing application used as a running example, illustrating how to implement it with patterns and frameworks in C++ and Java. It also compares object-oriented and non-object-oriented approaches, and highlights the use of scope, commonality, and variability analysis.

2.  **[Design Patterns Video Tutorial](https://www.youtube.com/watch?v=vNHpsC5ng_E)**

    This video by **Derek Banas** is a tutorial on **design patterns** using Java. It starts by reviewing fundamental object-oriented programming concepts such as inheritance, encapsulation, and the difference between instance and local variables, before diving into design patterns.
