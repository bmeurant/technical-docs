---
title: Tell, Don't Ask
tags:
  - design-principle
  - object-oriented
  - encapsulation
  - behavior
date: 2025-10-06
---
# Tell, Don't Ask

**Tell, Don't Ask** is a principle that helps to better encapsulate objects by reminding us that, instead of asking an object for its state and then making decisions based on that state, we should **tell** the object what to do. The object itself should contain the data and the logic to operate on that data.

This principle encourages moving logic to where the data lives, leading to higher cohesion and better encapsulation, which are cornerstones of [[object-oriented-programming|Object-Oriented Programming]].

---

## Core Idea: Shifting Responsibility

The core idea is to avoid pulling data out of an object to perform logic on it in another object. When you do this, the client object becomes responsible for logic that should belong to the object holding the data. This breaks encapsulation and tightly couples the client to the internal state and decision-making process of the other object.

-   **Asking (Violation):** `if (object.getStatus() == 'READY') { object.performAction(); }`
-   **Telling (Compliance):** `object.performActionIfReady();`

In the second case, the responsibility for checking the status is inside the object itself.

---

## Code Examples

### Violation: "Asking" for State

Imagine a `Thermostat` object that simply holds a temperature value. The main application logic asks for this temperature and then decides whether to adjust the heating.

```javascript
// The Thermostat object is just a data holder
class Thermostat {
    constructor() {
        this.currentTemperature = 18; // degrees Celsius
    }

    getCurrentTemperature() {
        return this.currentTemperature;
    }
}

// The main application logic
const thermostat = new Thermostat();
const desiredTemperature = 21;

// The client ASKS for the state and makes a decision
if (thermostat.getCurrentTemperature() < desiredTemperature) {
    console.log('Turning the heater on...');
    // heater.turnOn();
}
```

**Why is this a problem?** The main application is now responsible for the heating logic. If the rules for heating change (e.g., considering the time of day), this client code has to be modified. It is coupled to the thermostat's state.

### Application: "Telling" the Object to Act

To comply with the principle, we move the decision-making logic inside the `Thermostat` object. We *tell* the thermostat to regulate itself.

```javascript
class Thermostat {
    constructor() {
        this.currentTemperature = 18;
        this.desiredTemperature = 21;
    }

    // The client TELLS the thermostat to do its job.
    regulate() {
        if (this.currentTemperature < this.desiredTemperature) {
            this.turnHeaterOn();
        }
    }

    turnHeaterOn() {
        console.log('Turning the heater on...');
        // heater.turnOn();
    }
}

// The client code is now much simpler.
const thermostat = new Thermostat();
thermostat.regulate(); // Tell, don't ask!
```
Now, the client is completely decoupled from the heating logic. The `Thermostat` is the expert on its own state and is responsible for making decisions based on it. The client simply tells it what high-level action to perform.

---

## Benefits

-   **Stronger [[object-oriented-programming|Encapsulation]]:** Logic and the data it operates on are kept together in the same object.
-   **Reduced [[cohesion-coupling|Coupling]]:** The client code does not depend on the internal state or decision-making logic of other objects.
-   **Improved Maintainability:** The logic for a specific behavior is located in one place (the object that owns the data), making it easier to find, understand, and modify.

---

## Relationship with Other Principles

-   **[[law-of-demeter|The Law of Demeter (LoD)]]:** "Tell, Don't Ask" is the philosophical foundation for the Law of Demeter. LoD provides the concrete rules (don't chain method calls) that often lead to code that naturally follows the "Tell, Don't Ask" principle. If you have to ask for an object to then ask for another object, you are almost certainly breaking both principles.
-   **[[solid|Single Responsibility Principle (SRP)]]:** This principle supports the SRP by ensuring that the responsibility for a decision lies with the object that has the data required to make that decision (the "Information Expert").

---

## Resources & links

These resources provide more depth on the "Tell, Don't Ask" principle.

### Articles

1.  **[TellDontAsk by Martin Fowler](https://martinfowler.com/bliki/TellDontAsk.html)**
    A classic blog post from Martin Fowler explaining the core idea of moving logic to the data it operates on, thereby improving encapsulation.

2.  **[Tell, Don't Ask](https://deviq.com/principles/tell-dont-ask)**
    This article from DevIQ provides clear examples of violating the principle and how to refactor the code to comply with it, leading to better object-oriented design.

### Videos

1.  **[Tell, Don't Ask!](https://www.youtube.com/watch?v=AcYcbBVmZew)**
    In this short video, Steve Smith (Ardalis) explains how encapsulating logic with the state it operates on leads to less verbose and more consistent systems.

2.  **[A Fresh Look at Tell, Don't Ask](https://www.youtube.com/watch?v=ZJ6bH6Df4K8)**
    A talk by Ben Orenstein that provides a more nuanced and evolved perspective on the "Tell, Don't Ask" principle, discussing when and why it's most effective.
