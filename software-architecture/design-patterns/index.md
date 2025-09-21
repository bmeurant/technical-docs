---
title: Design Patterns
---

# Design Patterns

A design pattern is a reusable solution to a commonly occurring problem within a specific context of software design. It's a template, not a finished design, that you can adapt to your particular needs. Design patterns aren't code; they're descriptions or blueprints for how to solve a problem.

---

## Definition and Description

Design patterns represent a formalized best practice that a software developer can use to solve a common problem. Think of them as a set of instructions on how to structure code to be more flexible, maintainable, and understandable. They are typically categorized into three main types: **creational** (patterns that deal with object creation mechanisms), **structural** (patterns that explain how to compose classes and objects to form larger structures), and **behavioral** (patterns that are concerned with algorithms and the assignment of responsibilities between objects). A classic example is the **Singleton pattern**, a creational pattern that ensures a class has only one instance and provides a global point of access to it.

---

## Differences with Other Software Patterns

While "design pattern" is a broad term, it's often used more specifically to refer to the patterns described in the influential book *Design Patterns: Elements of Reusable Object-Oriented Software* by the "Gang of Four." These patterns are distinct from other software patterns, such as **[[software-architecture/architectural-patterns/|architectural patterns]]**, primarily in their **scope** and **level of abstraction**.

* **Design patterns** operate at a **lower level of abstraction**, focusing on the relationships and interactions between classes and objects within a single module or part of a system. They are about the detailed design of software components. Other examples are described in the [[posa|POSA]] series of books.
* **[[software-architecture/architectural-patterns/|Architectural patterns]]**, on the other hand, operate at a **higher level of abstraction** and concern the fundamental structural organization of a software system. They define the overall framework and skeleton of an application. Examples include the **[[mvc|Model-View-Controller (MVC)]]** pattern or the **[[client-server|Client-Server]]** pattern. They dictate how the major components of a system interact, not how individual classes are structured.

In essence, architectural patterns define the large-scale structure, while design patterns refine the internal structure of those larger components.
