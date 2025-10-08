---
title: The Law of Demeter (LoD)
tags:
  - design-principle
  - coupling
  - encapsulation
  - object-oriented
date: 2025-10-05
---
# The Law of Demeter (LoD)

The **Law of Demeter (LoD)**, also known as the **Principle of Least Knowledge**, is a design guideline for developing software, particularly [[object-oriented-programming|object-oriented systems]]. The core idea is that a module should have limited knowledge of other modules and should only interact with its "immediate friends."

This principle helps to reduce coupling between components, leading to systems that are more maintainable, adaptable, and less fragile. The law is often summarized as:

> "Only talk to your immediate friends."

---

## The Rules of the Law

For a method `M` belonging to an object `O`, the methods called within `M` should only belong to:

1.  The object `O` itself.
2.  Any objects passed as parameters to `M`.
3.  Any objects created or instantiated within `M`.
4.  The direct component objects of `O` (i.e., its instance variables).

Crucially, a method should **not** invoke methods on an object that was returned by another method call. This leads to long chains of method calls, a code smell often called a "train wreck."

---

## Code Examples

The Law of Demeter is best understood with a practical example.

### Violation: The "Train Wreck"

Imagine a `Customer` object that has a `Wallet`, which in turn has `Money`. Code that needs to get the final amount might look like this:

```javascript
// Non-compliant code that violates the Law of Demeter
class Customer {
    constructor() {
        this.wallet = new Wallet();
    }

    getWallet() {
        return this.wallet;
    }
}

class Wallet {
    constructor() {
        this.money = new Money(100);
    }

    getMoney() {
        return this.money;
    }
}

class Money {
    constructor(amount) {
        this.amount = amount;
    }

    getAmount() {
        return this.amount;
    }
}

// The client code
const customer = new Customer();
// This is a "train wreck" -> customer.getWallet().getMoney().getAmount()
const amount = customer.getWallet().getMoney().getAmount();
console.log(`Amount: ${amount}`);
```

**Why is this a problem?** The client code is now tightly coupled to the internal structure of `Customer`, `Wallet`, and `Money`. If the `Wallet` class is refactored to no longer contain a `Money` object directly, the client code will break, even though it only wanted to interact with the `Customer`.

### Application: Hiding the Structure

To comply with the Law of Demeter, the `Customer` class should provide a high-level method that hides its internal structure. The client should tell the customer what it wants, not how to get it.

```javascript
// Compliant code that follows the Law of Demeter
class Customer {
    constructor() {
        this.wallet = new Wallet();
    }

    // The Customer exposes a high-level operation.
    // It delegates the work to its "immediate friend" (this.wallet).
    getAvailableFunds() {
        return this.wallet.getTotalMoney();
    }
}

class Wallet {
    constructor() {
        this.money = new Money(100);
    }

    // The Wallet also delegates to its friend (this.money).
    getTotalMoney() {
        return this.money.getAmount();
    }
}

class Money {
    constructor(amount) {
        this.amount = amount;
    }

    getAmount() {
        return this.amount;
    }
}

// The client code is now simple and decoupled.
const customer = new Customer();
const amount = customer.getAvailableFunds(); // Just talk to the immediate friend!
console.log(`Amount: ${amount}`);
```
Now, the client is completely decoupled from the internal implementation of `Customer`. The `Wallet` or `Money` classes can be refactored freely without any impact on the client code.

---

## Benefits & Downsides

**Benefits:**

-   **[[cohesion-coupling|Loose Coupling]]:** This is the primary benefit. Components are less dependent on the internal structure of other components.
-   **Improved Maintainability:** Changes to a class's internal logic do not ripple through the system, as long as its public interface remains the same.
-   **Enhanced Encapsulation:** It forces objects to hide their implementation details and expose only high-level, meaningful operations.
-   **Increased Reusability:** Components with a well-defined, minimal interface are easier to reuse, a core idea of [[component-principles|component design principles]].

**Downsides:**

-   **Wrapper Class Explosion:** Strict adherence can lead to a large number of small wrapper or delegating methods, which can increase verbosity and add a layer of indirection that is sometimes unnecessary.
-   **Performance Overhead:** The additional method calls can introduce a small, though usually negligible, performance cost.

---

## Relationship with Other Principles

-   **[[encapsulate-what-varies|Encapsulation]] & [[object-oriented-programming|Object-Oriented Programming]]:** The Law of Demeter is a powerful tool for enforcing **Encapsulation**, a core pillar of OOP. By preventing objects from exposing their internal structure, it forces a clear separation between an object's public interface and its private implementation. This is a direct application of the [[encapsulate-what-varies|Encapsulate What Varies]] principle.
-   **[[soc|Separation of Concerns]]:** By enforcing strict encapsulation, LoD helps in achieving a better separation of concerns, where a component does not need to know the internal workings of another.
-   **[[solid|Single Responsibility Principle (SRP)]]:** By encouraging objects to expose high-level behaviors rather than their internal components, LoD naturally leads to classes that are more cohesive and focused on a single responsibility.
-   **[[tell-dont-ask|Tell, Don't Ask]]:** The Law of Demeter is closely related to the "Tell, Don't Ask" principle, which states that you should tell objects what to do rather than asking them for their state and then making decisions based on that state.

---

## Resources & links

These resources provide more depth on the Law of Demeter and its practical applications.

### Articles

1.  **[Mastering the Law of Demeter: A Tester's Guide to Code Excellence](https://daniel-delimata.medium.com/mastering-the-law-of-demeter-a-testers-guide-to-code-excellence-52b91df658ad)**
    This article explains the Law of Demeter as a design guideline for improving modularity and reducing coupling, with a focus on how it enhances testability.

2.  **[Law of Demeter in Java - Principle of Least Knowledge](https://www.geeksforgeeks.org/java/law-of-demeter-in-java-principle-of-least-knowledge/)**
    A practical guide from GeeksforGeeks that details the core principles of LoD with Java examples to illustrate how to avoid "train wreck" code.

### Videos

1.  **[Lesson 57 - The Law of Demeter](https://www.youtube.com/watch?v=L60bB4ekIpI)**
    In this lesson, software architect Mark Richards explains what the Law of Demeter is and provides clear examples of how to apply it to create loosely coupled systems.

2.  **[Clean Code - The Law of Demeter (Make Your Code Resilient)](https://www.youtube.com/watch?v=ad9aHgO907g)**
    This video provides a balanced look at the Law of Demeter, explaining its pros and cons and how to apply it to make your code more resilient to change.
