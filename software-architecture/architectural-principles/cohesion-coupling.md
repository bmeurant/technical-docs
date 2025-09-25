---
title: Cohesion & Coupling
tags:
  - quality
  - maintanability
date: 2025-09-17
---
# Cohesion & Coupling

In software engineering, **cohesion** and **coupling** are two fundamental [[software-architecture/architectural-principles/|architectural principles]] used to evaluate the quality of an architecture. Together, they determine a system's robustness and flexibility. The goal of good design is to achieve **high cohesion** and **low coupling**.

| **Concept** | **Definition** | **Analogy** | **Goal** |
| :--- | :--- | :--- | :--- |
| **Cohesion** | A measure of how well a module's internal elements are united to accomplish a single, well-defined task. | The unity of a football team. Each player has a specific function that contributes to the common goal. | **High** |
| **Coupling** | The degree of interdependence between different modules. | The connections between a train's cars. Strong links (high coupling) can limit flexibility. | **Low** |

---

## 1. Cohesion: The "Inner Strength" of a Module

Cohesion is a qualitative measure of a module's focus. The more a module has one unique responsibility, the stronger its cohesion. A highly cohesive module is easier to understand, test, and reuse.

### Cohesion Levels (from best to worst):

1.  **Functional Cohesion (Ideal)**: All parts of the module are essential to performing a single, well-defined task. This is the essence of the [[solid#Single Responsibility Principle (SRP)|Single Responsibility Principle (SRP)]].
    * **Example**: An `EmailValidator` class with a single `validate(emailAddress)` method.

2.  **Sequential Cohesion**: The output from one element of the module serves as the input to the next. The elements are dependent in a sequence of operations.
    * **Example**: A `processFile` function that first reads a file and then parses its content in memory.

3.  **Communicational Cohesion**: Elements all operate on the same set of data. Their actions are distinct but share the same data context.
    * **Example**: A `UserProfileManager` class that contains methods like `loadProfile()`, `validateProfileData()`, and `updateLastLoginDate()`. All operate on a user's profile data.

4.  **Procedural Cohesion**: Elements are grouped to follow a specific order of execution but have no data link.
    * **Example**: An `OrderProcessor` module that executes `checkInventory()`, `processPayment()`, and `sendConfirmationEmail()` in that specific order.

5.  **Temporal Cohesion**: Elements are grouped because they are executed at the same time (e.g., at system startup or shutdown), without any functional or data relationship.
    * **Example**: An `initializeApplication` method that opens a database connection, loads configuration settings, and initializes a logger.

6.  **Logical Cohesion**: Elements perform similar tasks, but the specific task to be executed is selected by a parameter.
    * **Example**: A `processData(dataType)` function where `dataType` can be 'XML' or 'JSON' to select the action to perform.

7.  **Coincidental Cohesion (The worst)**: The elements of the module have absolutely no relationship with each other. They are grouped by chance.
    * **Example**: A `Utilities` module containing functions for date validation, currency conversion, and array sorting.

---

## 2. Coupling: The "Interdependence" of Modules

Coupling measures the degree of interdependence between modules. Low coupling is a key goal because it makes modules more independent. A change in a loosely coupled module has little impact on others, which facilitates maintenance, debugging, and evolution.

### Coupling Levels (from best to worst):

1.  **Data Coupling (Ideal)**: Modules exchange only the necessary data to perform their task, through simple parameters.
    * **Example**: An `OrderService` class calling a `calculateTax(subtotal, taxRate)` method in a `TaxCalculator` class.

2.  **Stamp Coupling**: A module passes an entire data structure (an object) to another module, even if the latter only needs part of it. This creates a dependency on the data structure itself.
    * **Example**: `UserProfileService` calls `updateAddress(user)` where the entire `user` object is passed, but the function only uses the address.

3.  **Control Coupling**: A module passes control information (a flag, an enum) to direct the internal logic of the called module. The calling module "knows" how the called module works.
    * **Example**: A `processFile(file, mode)` method where `mode` determines if the file should be read in `read-only` or `read-write` mode.

4.  **External Coupling**: Modules depend on a common external factor (e.g., a file format, a communication protocol, a shared database).
    * **Example**: Two [[microservices|microservices]] that both depend on the same version of a database schema.

5.  **Common Coupling**: Modules share a global state, like a global variable. This is extremely dangerous because any module can modify this data, making the system unpredictable.
    * **Example**: Two modules that both read and write to a global `application_state` variable.

6.  **Content Coupling (The worst)**: A module modifies another module's internal data (e.g., by directly accessing a private variable). This is a total violation of encapsulation.
    * **Example**: Module A directly modifies a field in Module B without going through a public interface (setter).

---

## 3. The Relationship Between the Two: A Key Balance

Cohesion and coupling are inversely related and are two sides of the same architectural coin.

* **High Cohesion → Low Coupling**: A module with a single, well-defined responsibility is autonomous. It needs less information from the outside and interacts with others in a simple way, which reduces dependencies.
* **Low Cohesion → High Coupling**: A module that does many unrelated things often needs to rely on other modules to perform its various tasks, creating complex and fragile dependencies.

Striving for high cohesion and low coupling leads to systems that are **maintainable** (bugs are easier to isolate), **reusable** (modules are independent units that can be used elsewhere), and **scalable** (changes are localized). As an architect, the goal is to design modules that are like "black boxes": you know what they do (**high cohesion**) and how to interact with them (**low coupling**), without worrying about their internal workings.

---

## Links and Resources

To dive deeper into the concepts of cohesion and coupling in software engineering, here is an updated list of resources.

#### Articles

1. **[Coupling and Cohesion - Software Engineering](https://www.geeksforgeeks.org/software-engineering/software-engineering-coupling-and-cohesion/)** by GeeksforGeeks

    This article defines coupling and cohesion, explains their importance, and outlines the different types of each concept.

2. **[Cohesion and Coupling in Software with Examples](https://thevaluable.dev/cohesion-coupling-guide-examples/)** by The Valuable Dev

    A detailed guide that explores the origin of these concepts, their importance, and how to apply them with practical examples.

#### Videos

1. **[SOLID Principles? Nope, just Coupling and Cohesion](http://www.youtube.com/watch?v=YDNR_gfBk0Q)** by CodeOpinion

    This video highlights the importance of coupling and cohesion, sometimes even beyond the well-known [[solid|SOLID principles]].

2. **[Lesson 59 - The Tradeoffs of Loose Coupling](https://www.youtube.com/watch?v=XnBhVwm_Lws)** by Mark Richards

    This video discusses the tradeoffs of loose coupling, using a [[microservices|microservices]] example to illustrate the challenges of managing errors and data consistency.

3. **[Coupling and Cohesion Explained](http://www.youtube.com/watch?v=7pdrZDqEPIw)** by Gui Ferreira

    This video provides a clear and concise explanation of these two fundamental software design principles and their importance in creating maintainable code.