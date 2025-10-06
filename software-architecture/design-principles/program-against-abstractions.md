---
title: Program against Abstractions
tags:
  - design-principle
  - abstraction
  - decoupling
  - solid
  - dependency-inversion
date: 2025-10-06
---
# Program to an Interface, Not an Implementation

This principle is a cornerstone of robust [[object-oriented-programming|object-oriented design]]. It states that your code should depend on abstract types (interfaces or abstract classes) rather than concrete implementations. By doing so, you decouple your code from specific implementations, making the system more flexible, maintainable, and easier to test.

This principle is the core idea behind the **[[solid|Dependency Inversion Principle]]**.

---

## Key Concepts

-   **Abstraction (The "Interface"):** In this context, an "interface" is not just a language keyword (like `interface` in Java or C#). It represents any abstract type that defines a contract—a set of methods and properties—without specifying how they are implemented. This could be an actual interface, an abstract class, or even a function signature in a dynamically typed language.
-   **Concrete Implementation:** This is the actual code that fulfills the contract defined by the abstraction. It's the "how" to the interface's "what."

By programming to an interface, you are saying that your code will only interact with the contract, not with the specific details of any one implementation.

---

## Benefits

-   **[[cohesion-coupling|Loose Coupling]]:** This is the primary benefit. The client code is not tied to a specific implementation. You can change the implementation at any time without affecting the client, as long as the new implementation adheres to the interface's contract.
-   **Flexibility and Extensibility:** New implementations can be added to the system without modifying the client code. This is a direct application of the **[[solid|Open/Closed Principle]]**.
-   **Improved Testability:** When code depends on abstractions, it is much easier to test. In your unit tests, you can provide a "mock" or "stub" implementation of the interface, allowing you to test the client code in isolation.
-   **Enhanced Readability and Maintainability:** Code that depends on abstractions is often easier to understand because it focuses on the high-level behavior (the "what") rather than the low-level details (the "how").

---

## Code-Level Illustration

Let's consider a notification system.

### Violation: Programming to an Implementation

In this example, the `NotificationService` is directly coupled to the `EmailNotifier` class.

```javascript
// The concrete implementation
class EmailNotifier {
    send(message) {
        console.log(`Sending email: ${message}`);
    }
}

// The high-level service is directly dependent on the concrete class
class NotificationService {
    constructor() {
        this.notifier = new EmailNotifier(); // Tight coupling!
    }

    sendNotification(message) {
        this.notifier.send(message);
    }
}

const service = new NotificationService();
service.sendNotification("Hello, World!");
```

**Why is this a problem?** If you want to add a new notification method, like SMS, you have to modify the `NotificationService` class. What if you want to use a different email service? You have to change the `NotificationService` again. The service is not closed for modification.

### Application: Programming to an Interface

Here, the `NotificationService` depends on a `Notifier` interface (in JavaScript, this can be an implicit contract).

```javascript
// The Abstraction (an implicit interface in this case)
// We expect any notifier to have a `send(message)` method.

// Concrete Implementations
class EmailNotifier {
    send(message) {
        console.log(`Sending email: ${message}`);
    }
}

class SmsNotifier {
    send(message) {
        console.log(`Sending SMS: ${message}`);
    }
}

// The high-level service depends on the abstraction, which is injected.
class NotificationService {
    constructor(notifier) {
        this.notifier = notifier; // Depends on the abstraction
    }

    sendNotification(message) {
        this.notifier.send(message);
    }
}

// The client can now choose the implementation at runtime.
const emailService = new NotificationService(new EmailNotifier());
emailService.sendNotification("Hello via Email!");

const smsService = new NotificationService(new SmsNotifier());
smsService.sendNotification("Hello via SMS!");
```

Now, the `NotificationService` is completely decoupled from the concrete notifier classes. We can add as many new notifier types as we want without ever changing the `NotificationService`. This is a flexible and maintainable design.

---

## Relationship with Other Principles

-   **[[solid|Dependency Inversion Principle (DIP)]]:** This principle is the formalization of "program to an interface." The DIP states that high-level modules should not depend on low-level modules; both should depend on abstractions.
-   **[[solid|Open/Closed Principle (OCP)]]:** Programming to an interface is a key enabler of the OCP. It allows you to extend the behavior of a system (by adding new implementations) without modifying existing code.
-   **[[composition-over-inheritance|Composition over Inheritance]]:** This principle often goes hand-in-hand with programming to an interface. Objects are composed of other objects, and the relationship is defined by the interfaces they implement, not by their concrete classes.
-   **[[gof|Design Patterns]]:** Many design patterns are built on this principle.
    -   The **Strategy** pattern uses interfaces to define a family of interchangeable algorithms.
    -   The **Abstract Factory** pattern provides an interface for creating families of related objects.
    -   The **Bridge** pattern decouples an abstraction from its implementation using an interface.
-   **Architectural Patterns**: Many architectural patterns rely on this principle to achieve decoupling.
    -   In a **[[layered|Layered Architecture]]**, layers communicate with each other through abstract interfaces, hiding their internal implementations.
    -   A **[[modular-monolith|Modular Monolith]]** is composed of modules that expose public interfaces and hide their implementation details.
    -   **[[component-based|Component-Based Architecture]]** is fundamentally based on components that expose well-defined interfaces.
-   **[[hollywood-principle|The Hollywood Principle (IoC)]]**: Inversion of Control is often implemented by depending on abstractions. A framework or container (the one "calling you") interacts with your code through interfaces that your components implement.

---

## Resources & links

These resources offer further insights into the "Program to an Interface" principle and how to apply it effectively.

### Articles

1.  **[Programming to Interface Vs to Implementation](https://dmitripavlutin.com/interface-vs-implementation/)**
    A detailed blog post by Dmitri Pavlutin that uses clear code examples to demonstrate the drawbacks of programming to an implementation and the benefits of using interfaces to create flexible, decoupled code.

2.  **[Program to interface NOT implementation](https://medium.com/@essaadani.yo/program-to-interface-not-implementation-29154de5c5b4)**
    This article provides a concise explanation of the principle, focusing on how it improves maintainability and scalability by reducing dependencies between components.

### Videos

1.  **[Program to an Interface, Not an Implementation](https://www.youtube.com/watch?v=h8pm8MMNmho)**
    A clear and concise video that explains the concept using a simple analogy and code examples, making it easy to understand for beginners.

2.  **[Always Use Interfaces](https://www.youtube.com/watch?v=TkUhAbbRCAE)**
    This video makes a strong case for using interfaces as a default practice, explaining how it leads to better software design and more maintainable code in the long run.
