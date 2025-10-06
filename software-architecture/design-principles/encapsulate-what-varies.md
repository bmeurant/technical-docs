---
title: Encapsulate What Varies
tags:
  - design-principle
  - encapsulation
  - change-management
  - solid
  - open-closed-principle
date: 2025-10-06
---
# Encapsulate What Varies

This principle is a simple but powerful idea that is at the heart of many design patterns and principles. It can be stated as:

> "Identify the aspects of your application that vary and separate them from what stays the same."

The goal is to minimize the impact of change. By isolating the parts of your system that are likely to change, you can modify or extend them without affecting the stable parts of your codebase.

---

## Key Concepts

-   **Identify Variability:** The first step is to carefully analyze your system and identify the parts that are likely to change. This could be anything: business rules, algorithms, data formats, external dependencies, or UI components.
-   **Encapsulate and Isolate:** Once you've identified a variable aspect, you should encapsulate it behind a stable [[program-against-abstractions|interface]]. This creates a boundary that protects the rest of the system from changes in the encapsulated part. The rest of the system interacts with the variable part only through this stable interface.

---

## Benefits

-   **Improved Maintainability:** When a change is required, it is localized to a single, well-defined area of the code. This makes the system much easier to maintain and reduces the risk of introducing bugs.
-   **Enhanced Flexibility and Extensibility:** The system is more flexible because the variable parts can be easily swapped out or extended with new implementations without affecting the stable parts.
-   **Reduced Impact of Change:** By isolating the parts that are likely to change, you minimize the ripple effect of those changes across the codebase. A change in one area is less likely to break another.

---

## Code-Level Illustration

Let's consider an e-commerce application that needs to calculate shipping costs. Shipping rules often change based on the carrier, the destination, or the weight of the package.

### Violation: No Encapsulation

In this example, the shipping logic is hard-coded directly inside the `Order` class.

```javascript
class Order {
    constructor(items, shippingMethod) {
        this.items = items;
        this.shippingMethod = shippingMethod;
    }

    getTotalCost() {
        const itemCost = this.items.reduce((total, item) => total + item.price, 0);
        let shippingCost = 0;

        // This part is likely to vary
        if (this.shippingMethod === 'FedEx') {
            shippingCost = 10; // Simplified logic
        } else if (this.shippingMethod === 'UPS') {
            shippingCost = 12;
        } else {
            shippingCost = 8; // Standard shipping
        }

        return itemCost + shippingCost;
    }
}
```

**Why is this a problem?** Every time a new shipping method is added or an existing one is changed, you have to modify the `Order` class. This violates the **[[solid|Open/Closed Principle]]**.

### Application: Encapsulating the Variable Part

Here, we encapsulate the shipping calculation logic into separate classes that all implement a common interface.

```javascript
// The Abstraction (the stable interface)
class ShippingCalculator {
    calculateCost(order) {
        throw new Error("This method should be overridden!");
    }
}

// Concrete Implementations (the variable parts)
class FedExCalculator extends ShippingCalculator {
    calculateCost(order) {
        return 10; // Simplified logic
    }
}

class UpsCalculator extends ShippingCalculator {
    calculateCost(order) {
        return 12;
    }
}

// The Order class now depends on the abstraction
class Order {
    constructor(items, shippingCalculator) {
        this.items = items;
        this.shippingCalculator = shippingCalculator;
    }

    getTotalCost() {
        const itemCost = this.items.reduce((total, item) => total + item.price, 0);
        const shippingCost = this.shippingCalculator.calculateCost(this);
        return itemCost + shippingCost;
    }
}

// The client can now create an order with any shipping calculator
const fedexCalculator = new FedExCalculator();
const order1 = new Order([{ price: 50 }], fedexCalculator);
console.log(order1.getTotalCost()); // Outputs 60

const upsCalculator = new UpsCalculator();
const order2 = new Order([{ price: 50 }], upsCalculator);
console.log(order2.getTotalCost()); // Outputs 62
```

Now, the `Order` class is stable. If we need to add a new shipping method, we just create a new `ShippingCalculator` implementation. The `Order` class doesn't need to change.

---

## Relationship with Other Principles

-   **[[solid|Open/Closed Principle (OCP)]]:** This principle is a direct way to achieve the OCP. By encapsulating what varies, you create a system that is open for extension (you can add new implementations of the variable part) but closed for modification (the stable part of the system doesn't need to change).
-   **[[program-against-abstractions|Program to an Interface, Not an Implementation]]:** To encapsulate what varies, you almost always need to program to an interface. The interface represents the stable contract, while the concrete classes behind it are the variable implementations.
-   **[[soc|Separation of Concerns (SoC)]]:** Encapsulating what varies is a form of SoC. You are separating the concern of "things that change" from the concern of "things that stay the same".
-   **[[gof|Design Patterns]]:** This is the core idea behind many design patterns.
    -   The **Strategy** pattern is the classic example of encapsulating what varies (the algorithm).
    -   The **Decorator** pattern allows you to add new behavior (a variable aspect) to an object dynamically.
    -   The **Bridge** pattern separates an abstraction from its implementation, allowing them to vary independently.

---

## Resources & links

These resources offer further insights into the "Encapsulate What Varies" principle and how to apply it effectively.

### Articles

1.  **[Encapsulate What Varies by Alex Kondov](https://alexkondov.com/encapsulate-what-varies/)**
    A concise and practical explanation of the principle, with a focus on how it helps to manage frequently changing requirements by isolating the parts of the code that are most likely to change.

2.  **[Encapsulate What Varies: A Foundational Principle in Software Design](https://medium.com/@Masoncoding/encapsulate-what-varies-a-foundational-principle-in-software-design-f0cca11131f2)**
    This article provides a deep dive into the principle, explaining its core characteristics and how it can be applied in different programming paradigms to create more flexible and maintainable systems.