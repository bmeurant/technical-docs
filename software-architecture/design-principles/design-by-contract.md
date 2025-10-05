---
title: Design by Contract (DbC)
tags:
  - design-principle
  - design-by-contract
  - robustness
  - defensive-programming
date: 2025-10-02
---
# Design by Contract (DbC)

**Design by Contract (DbC)** is a software design methodology that uses formal, precise, and verifiable interface specifications for software components. The core idea is that software components should cooperate based on a clear "contract," much like a business contract between a client and a supplier.

Introduced by Bertrand Meyer, the creator of the Eiffel programming language, DbC views software construction as being based on components whose interactions are governed by mutual obligations and benefits.

---

## The Client-Supplier Analogy

DbC is best understood through the lens of a client (a piece of code calling a method) and a supplier (the method being called). Their contract defines:

-   **The Supplier's Obligations:** What the supplier must provide (the result).
-   **The Supplier's Benefits:** What the supplier can assume to be true before starting its work (the inputs).
-   **The Client's Obligations:** What the client must provide to the supplier (the inputs).
-   **The Client's Benefits:** What the client can expect to receive (the result).

This relationship is formalized through three key concepts: **preconditions**, **postconditions**, and **invariants**.

---

## The Three Core Concepts

### 1. Preconditions

A precondition is a condition that must be true **before** a method is invoked. It is the client's responsibility to satisfy the precondition. If the precondition is not met, the supplier is not obligated to do anything—it is free to crash, produce incorrect results, or do nothing at all.

-   **Responsibility:** The **Client** (caller).
-   **Purpose:** Defines the valid inputs and state required for the method to run.

**Example (using assertions to check preconditions):**

```javascript
// A bank account that cannot have a negative balance.
class BankAccount {
    constructor(balance = 0) {
        if (balance < 0) throw new Error('Initial balance cannot be negative.');
        this.balance = balance;
    }

    /**
     * @param {number} amount The amount to withdraw.
     * @precondition amount > 0
     * @precondition this.balance >= amount
     */
    withdraw(amount) {
        // Precondition Check 1: Amount must be positive.
        if (amount <= 0) {
            throw new Error('Withdrawal amount must be positive.');
        }
        // Precondition Check 2: Sufficient funds must be available.
        if (this.balance < amount) {
            throw new Error('Insufficient funds.');
        }

        this.balance -= amount;
    }
}
```
In this case, the caller of `withdraw` is obligated to provide a positive `amount` that does not exceed the current balance.

### 2. Postconditions

A postcondition is a condition that the method **guarantees** will be true **after** it finishes executing successfully. It is the supplier's responsibility to satisfy the postcondition.

-   **Responsibility:** The **Supplier** (the method itself).
-   **Purpose:** Defines the outcome, results, and state changes the method promises to deliver.

**Example (checking postconditions):**

```javascript
class BankAccount {
    // ... (constructor and other methods)

    /**
     * @param {number} amount The amount to withdraw.
     * @postcondition this.balance == old(this.balance) - amount
     */
    withdraw(amount) {
        // Precondition checks...
        if (amount <= 0 || this.balance < amount) { /* ... throw errors ... */ }

        const balanceBefore = this.balance;

        // Action
        this.balance -= amount;

        // Postcondition Check: Ensure the balance was updated correctly.
        // In a language with native DbC, this would be automatic.
        // In JavaScript, we can use an assertion for testing/debugging.
        console.assert(this.balance === balanceBefore - amount, 'Postcondition failed: Balance incorrect after withdrawal.');
    }
}
```

### 3. Invariants

An invariant is a condition for a class that must be true **whenever the object is publicly accessible**. This means it must be true after the constructor has run and before and after every public method call. Invariants ensure the object remains in a consistent, valid state.

-   **Responsibility:** The **Supplier** (the class as a whole).
-   **Purpose:** Defines the fundamental consistency and integrity of an object.

**Example (checking an invariant):**

```javascript
class BankAccount {
    constructor(balance = 0) {
        this.balance = balance;
        this.checkInvariant(); // Check after creation
    }

    // The invariant: The balance can never be negative.
    checkInvariant() {
        if (this.balance < 0) {
            throw new Error('Invariant violated: Balance cannot be negative.');
        }
    }

    withdraw(amount) {
        this.checkInvariant(); // Check before method
        // Preconditions...

        this.balance -= amount;

        this.checkInvariant(); // Check after method
    }

    deposit(amount) {
        this.checkInvariant(); // Check before method
        // Preconditions...

        this.balance += amount;

        this.checkInvariant(); // Check after method
    }
}
```

---

## Benefits of Design by Contract

-   **Improved Reliability:** It forces developers to think rigorously about component responsibilities, leading to more robust code.
-   **Clear Documentation:** Contracts act as a precise form of technical documentation, explaining what a component does and how to use it.
-   **Enhanced Debugging:** When a contract is violated, the system fails immediately at the source of the error (the bug is in the client if a precondition fails, or the supplier if a postcondition fails), making debugging much faster. It is a systematic way to implement a **Fail-Fast** strategy.
-   **Facilitates Automated Testing:** Contracts can be used as a basis for generating test cases.

---

## Relationship with Other Concepts

-   **[[defensive-programming|Defensive Programming]]:** DbC is a formal and structured way to implement defensive checks. While general defensive programming might handle errors gracefully (e.g., returning a default value), DbC typically advocates for failing fast when a contract is broken, as a contract violation signals a bug.
-   **[[fail-fast|Fail-Fast]]:** DbC is one of the most systematic ways to implement a Fail-Fast strategy. A contract violation is a clear signal that the system is in an invalid state, and execution should be halted immediately.
-   **[[solid|Liskov Substitution Principle (LSP)]]:** DbC provides the rules for applying LSP correctly in object-oriented programming. For a subclass to be a valid substitute for its superclass, it must adhere to the parent's contract:
    -   It must not strengthen the preconditions (it must accept at least everything the parent accepts).
    -   It must not weaken the postconditions or the invariants (it must guarantee at least everything the parent guarantees).

---

## Resources & links

These resources provide more depth on the theory and practice of Design by Contract.

### Articles

1.  **[Design by Contract and Assertions](https://www.eiffel.org/doc/solutions/Design_by_Contract_and_Assertions)**
    A foundational text from eiffel.org, the home of DbC's creator. It explains how contracts are embedded into code using assertions to improve reliability.

2.  **[Design by contracts (in Ada)](https://learn.adacore.com/courses/intro-to-ada/chapters/contracts.html)**
    This article provides a practical look at implementing preconditions, postconditions, and invariants in the Ada programming language, which has native support for contracts.

### Videos

1.  **[Defensive Programming and Design By Contract](https://www.youtube.com/watch?v=PG-c2vJ0Woo)**
    This video explains how DbC serves as a key strategy for building robust code that can detect and handle bad data, using asserts and frameworks.

2.  **[Let Design by Contract Be Your Highest-Valued Design Method](https://www.youtube.com/watch?v=E30N4FJwP3w)**
    A talk that argues for the value of DbC in everyday class design to prevent common bugs, drawing parallels to service contracts in microservices.