--- 
title: Declarative Programming
tags:
  - programming-paradigm
date: 2025-09-28
---
# Declarative Programming

In contrast to the [[imperative-programming|imperative paradigm]], which focuses on **how** to achieve a result, the **declarative programming** paradigm focuses on **what** the program should accomplish, without specifying the control flow. You describe the desired result, and the language's execution engine is responsible for figuring out the steps to get there.

An excellent analogy is asking for directions. An imperative approach would be a turn-by-turn list of instructions ("drive 2 miles, turn left, drive 300 feet..."). A declarative approach would be simply stating the destination address and letting a GPS device (the engine) calculate the route.

This paradigm is built on core concepts like:
- **Focus on Logic:** You express the logic of the computation, not the step-by-step execution.
- **Immutability:** State changes are often avoided. Instead of modifying data, new data structures are created from existing ones.
- **No Explicit Control Flow:** You typically do not use loops (`for`, `while`) or complex conditional branching. The underlying engine handles the iteration and flow.

**Main Declarative Paradigms**: Declarative programming is primarily expressed through two major sub-paradigms: **Functional Programming** and **Logical Programming**.

```mermaid
graph TD
    A[Declarative Programming] --> B[Functional Programming];
    A --> C[Logical Programming];
```

## 1. Functional Programming

Functional Programming (FP) treats computation as the evaluation of mathematical functions. It emphasizes the use of **pure functions** and **immutable data**. A pure function is one whose output value depends only on its input values, without any observable side effects (like modifying a global variable or writing to a file).

### Key Characteristics
- **Pure Functions:** Predictable and easy to test, as they have no hidden state or side effects.
- **Immutability:** Data is never changed in place. This eliminates a whole class of bugs related to [[transversal-programming-models#1-concurrent-programming|concurrent]] access and unexpected state changes.
- **First-Class Functions:** Functions are treated like any other data type. They can be passed as arguments to other functions, returned as values, and stored in variables.
- **Composition:** Complex behavior is built by composing simpler functions together, much like a [[pipe-filters|pipeline]].
- **Examples of Languages:** Haskell, Lisp, F#, Clojure. Many modern languages like JavaScript, Python, and C# have incorporated strong functional features.

These concepts are explored in greater detail on the dedicated [[functional-programming|Functional Programming]] page.

This emphasis on purity and immutability naturally leads to systems with very high [[cohesion-coupling|cohesion]] and low [[cohesion-coupling|coupling]].

## 2. Logical Programming

Logical Programming is based on the principles of formal logic. A program in this paradigm is a collection of **facts** and **rules**. Execution involves posing a **query** to the system, which then uses a process of logical deduction to determine if the query is true or false based on the existing knowledge base. If the query contains variables, the system will attempt to find all possible values that make the query true.

### Key Characteristics
- **Facts and Rules:** The program is a database of logical statements (e.g., "Socrates is a human," "All humans are mortal").
- **Queries:** The program is executed by asking a question (e.g., "Is Socrates mortal?").
- **Unification and Backtracking:** The engine works by trying to match the query against the facts and rules. If a path fails, it can automatically backtrack and try another one.
- **Examples of Languages:** Prolog, Datalog.

## Key Differences Summarized

| Characteristic | Functional Programming | Logical Programming |
| :--- | :--- | :--- |
| **Based On** | Mathematical Functions | Formal Logic & Relations |
| **Core Idea** | Composing functions to transform data | Proving theorems based on facts and rules |
| **Execution Model** | Function application and reduction | Querying, unification, and backtracking |
| **Primary Use Case** | Data processing, concurrency, mathematical computation | Artificial intelligence, expert systems, database querying |

In summary, declarative programming provides a higher level of abstraction that allows developers to focus on the problem's logic rather than the machine's execution details. Functional programming achieves this through the composition of pure functions, while logical programming uses the power of formal deduction.

## **Resources & Links**

### Articles

1. **[Declarative programming: When “what” is more important than “how”](https://www.ionos.com/digitalguide/websites/web-development/declarative-programming/)** (IONOS)

    A programming paradigm focused on describing the **desired end result** (the "**what**") rather than the execution steps (the "**how**"). Characterized by a high level of **Abstraction**, **short, efficient code**, and reliance on an **algorithm** (Functional or Logic programming) to automatically determine the solution path.

2. **[I finally understand Declarative Programming](https://blog.saihemanth.com/posts/I-Finally-Understand-Declarative/)** (Sai Hemanth's Blog)

    Defines the paradigm as writing code that closely matches the **human language explanation** of the solution, expressing **what** it does. It emphasizes that implementation details like **iteration** and **state mutation** (C++ `for` loops) must be hidden, favoring **composition** and **pure functions** (like in Haskell or SQL).
