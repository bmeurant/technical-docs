---
title: Programming Paradigms
date: 2025-09-28
---
# Programming Paradigms

A programming paradigm is a fundamental style or way of thinking about and building the structure of a computer program. It provides a set of concepts and principles that shape how we write code, from the overall architecture down to individual expressions.

This documentation organizes programming paradigms into three main categories, as illustrated below.

```mermaid
graph TD
    A[Programming Paradigms] --> B["Imperative Programming"]
    A --> C["Declarative Programming"]
    A --> D["Transversal Models"]

    B --> B1[Procedural]
    B --> B2[Object Oriented]

    C --> C1[Functional]
    C --> C2[Logical]

    D --> D1[Concurrent Programming]
    D --> D2[Reactive Programming]
    D --> D3[Aspect Oriented Programming]
    D --> D4[Functional Reactive Programming]
```

## Imperative Programming

The imperative paradigm focuses on **how** to execute a task. It describes a sequence of commands that change the program's state.

[[imperative-programming|Learn more about Imperative Programming]]

- **Procedural:** Organizes code into procedures or functions that are called in a specific order.
- **[[object-oriented-programming|Object-Oriented]]:** Groups data (attributes) and the operations that modify that data (methods) into objects.

## Declarative Programming

The declarative paradigm focuses on **what** to execute. It describes the logic of a computation without specifying its control flow.

[[declarative-programming|Learn more about Declarative Programming]]

- **Functional:** Treats computation as the evaluation of mathematical functions and avoids changing-state and mutable data.
- **Logical:** Based on formal logic, where programs are sets of logical clauses and computation is a form of deduction.

## Transversal Programming Models

Transversal models are paradigms or concerns that can be combined with or applied across the main imperative and declarative paradigms.

[[transversal-programming-models|Learn more about Transversal Programming Models]]

- **Concurrent Programming:** Manages the simultaneous execution of multiple instruction sequences.
- **Reactive Programming:** Focuses on data flows and the propagation of change.
- **Aspect-Oriented Programming:** Separates cross-cutting concerns (like logging or security) from the core business logic.
- **Functional Reactive Programming:** A combination of functional and reactive programming to handle asynchronous data flows in a declarative way.

## **Resources & Links**

### Articles

1. **[Imperative vs Declarative Programming](https://ui.dev/imperative-vs-declarative-programming)** (ui.dev)

    Compares the two paradigms: **Imperative** focuses on ***how*** to do something (step-by-step instructions, often with state mutation, like vanilla JavaScript or jQuery) while **Declarative** focuses on ***what*** you want (describing the desired outcome, minimizing state mutation, like SQL, HTML, or **React**).

2. **[Imperative vs. Declarative Programming](https://www.fullstackfoundations.com/blog/imperative-vs-declarative)** (Full Stack Foundations)

    Explains the difference as **Imperative** = **HOW** (listing every step, low **Abstraction**, procedural code) versus **Declarative** = **WHAT** (specifying the desired output, high **Abstraction**, functional code). Illustrates with an example comparing imperative **jQuery** DOM manipulation to declarative **React** state management.

### Videos

3. **[Imperative vs Declarative Programming](http://www.youtube.com/watch?v=E7Fbf7R3x6I)** (ui.dev)

    Uses analogies and JavaScript examples (e.g., array iteration vs. **`.map()`** or **`.reduce()`**) to clarify that **Imperative** code describes **how** the machine operates, forcing a programmer to think step-by-step, whereas **Declarative** code describes **what** is desired, abstracting away the operational details.

---

4. **[Imperative vs Declarative Programming](http://www.youtube.com/watch?v=yOBBkIJBEL8)** (Tadas Petra)

    Defines **Imperative** as telling the application to perform actions (e.g., `set text`), while **Declarative** is setting a desired **state** and letting the underlying framework (like a modern UI library) handle the rebuild and transition to the new view. Shows a simple Dart example comparing a manual **`for loop`** to a declarative **`.where().toList()`** approach.