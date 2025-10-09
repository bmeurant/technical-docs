---
title: SOLID
tags:
  - design-principle
  - solid
  - object-oriented
  - dependency-inversion
date: 2025-09-16
---
# The SOLID Principles

The **SOLID** principles are a set of five [[software-architecture/design-principles/|design guidelines]] for software development that, when applied, help to create systems that are more maintainable, flexible, and scalable. Adhering to them is fundamental to achieving [[clean-code|Clean Code]]. It is an acronym for:

* **S** - Single Responsibility Principle (SRP)
* **O** - Open/Closed Principle (OCP)
* **L** - Liskov Substitution Principle (LSP)
* **I** - Interface Segregation Principle (ISP)
* **D** - Dependency Inversion Principle (DIP)

---

## 1. Single Responsibility Principle (SRP)

**The Principle:** A class should have only one reason to change, which means it should have only one responsibility or be responsible to a single "actor" (a user, a stakeholder, or another system). This keeps classes focused and cohesive, preventing them from becoming "God objects" that are difficult to manage.

**Example:** Imagine a class named `User` that is responsible for handling user data, sending email notifications, and generating PDF reports. This class violates the SRP because it has three reasons to change: if the user data structure changes, if the email service changes, or if the PDF report format changes. To adhere to the principle, these responsibilities should be separated into individual classes, such as `UserData`, `EmailService`, and `ReportGenerator`.

---

## 2. Open/Closed Principle (OCP)

**The Principle:** Software entities should be **open for extension**, but **closed for modification**. This means you should be able to add new functionality to a class without changing its existing source code.

**Example:** Consider a drawing application that needs to calculate the area of different shapes. A naive approach might be to use an `if-else` block to handle each shape type (`if shape is a circle, calculate area; else if shape is a square, calculate area`). To follow the OCP, you'd create an `IShape` interface with a single `calculateArea()` method. Each specific shape class (`Circle`, `Square`) would then implement this interface. This allows you to add a new shape, like a `Triangle`, without ever modifying the original area calculation logic.

---

## 3. Liskov Substitution Principle (LSP)

**The Principle:** Objects of a superclass should be replaceable with objects of its subclasses without affecting the correctness of the program. It ensures that inheritance is used correctly based on behavior rather than just a conceptual "is-a" relationship.

**Example:** Consider a base `Bird` class with a `fly()` method. It would seem logical to create a subclass `Ostrich` that inherits from `Bird`. However, a program that expects a `Bird` and calls the `fly()` method would fail or behave unpredictably when given an `Ostrich` object, as ostriches can't fly.  The `Ostrich`, as a subtype, violates the implicit contract of its parent. The LSP teaches us that if a subtype can't behave as its parent, the inheritance relationship is a poor design choice. This is a key reason to favor [[composition-over-inheritance|composition over inheritance]].

---

## 4. Interface Segregation Principle (ISP)

**The Principle:** Clients should not be forced to depend on interfaces they do not use. In simpler terms, it's better to have multiple small, specific interfaces than one large, generic interface.

**Example:** Imagine an `IVehicle` interface with methods like `drive()`, `fly()`, and `sail()`. A `Car` class, which only drives, would be forced to implement the `fly()` and `sail()` methods even though it has no use for them. This violates the ISP. The solution is to segregate the interface into smaller, more specific ones. You could have an `IDriveable` interface with a `drive()` method, an `IFlyable` with a `fly()` method, and an `ISailable` with a `sail()` method. The `Car` class would then only implement the `IDriveable` interface, depending only on what it actually needs. A more complex vehicle, like an amphibious car, could implement both `IDriveable` and `ISailable`.

---

## 5. Dependency Inversion Principle (DIP)

**The Principle:**
1. High-level modules should not depend on low-level modules. Both should depend on abstractions.
2. Abstractions should not depend on details. Details should depend on abstractions.

This is the formalization of the principle to [[program-against-abstractions|program to an interface, not an implementation]].

This principle is the key mechanism that enables the decoupling in domain-centric architectures like **[[hexagonal|Hexagonal]]**, **[[onion|Onion]]**, and **[[clean|Clean Architecture]]**.

**Example:** A high-level `PaymentService` module needs to log transaction details. A violation of the DIP occurs if `PaymentService` directly depends on a specific `FileLogger` class. If you want to change the logging method to a database or a cloud service, you must modify the `PaymentService` code. To follow the DIP, both the `PaymentService` and the `FileLogger` would depend on an `ILogger` interface. The `PaymentService` would use the `ILogger` interface, and the `FileLogger` (or a new `DatabaseLogger`) would implement it. This inverts the dependency, allowing you to change the low-level details without touching the high-level business logic. This inversion of control is a powerful concept, further explored in the [[hollywood-principle|Hollywood Principle]], where it is applied more broadly at an architectural level.

## **Resources & Links**

These resources will provide you with a solid foundation to master the SOLID principles and apply their concepts in your code.

### **Articles**

1.  **[SOLID Design Principles Explained: Building Better Software Architecture](https://www.digitalocean.com/community/conceptual-articles/s-o-l-i-d-the-first-five-principles-of-object-oriented-design)**

    This excellent resource from DigitalOcean explains the SOLID principles by focusing on building better software architecture. The article is very well-structured and uses clear code examples for each principle, making it easy to follow.

2.  **[SOLID Design Principles](https://sandimetz.com/blog/2009/03/21/solid-design-principles)**

    This article by Sandi Metz offers a clear and practical perspective on how SOLID principles integrate into daily code development, particularly with the use of Test-Driven Development (TDD).

---

### **Videos**

1.  **[Becoming a better developer by using the SOLID design principles](https://www.youtube.com/watch?v=rtmFCcjEgEw)**

    In this conference from Laracon EU, Katerina Trajchevska explains how SOLID principles can help you become a better developer by focusing on building clean and flexible software designs.

2.  **[SOLID Object-Oriented Design](http://videos.goruco.com/2009/sandi-metz-solid-object-oriented-design)**

    In this conference, Sandi Metz covers all the SOLID principles with a particular focus on how they improve code and simplify testing. It's an excellent resource for anyone looking for a practical approach.