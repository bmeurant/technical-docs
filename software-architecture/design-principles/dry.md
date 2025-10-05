---
title: Don't Repeat Yourself (DRY)
tags:
  - design-principle
  - dry
  - maintainability
  - soc
date: 2025-10-02
---
# Don't Repeat Yourself (DRY)

The **Don't Repeat Yourself (DRY)** principle is a cornerstone of software development, originating from the book *The Pragmatic Programmer* by Andy Hunt and Dave Thomas. It is defined as:

> "Every piece of knowledge must have a single, unambiguous, authoritative representation within a system."

This principle is not merely about avoiding duplicated code; it's about ensuring that each concept and piece of logic—the "knowledge"—is defined in only one place. This knowledge can be a business rule, an algorithm, a configuration constant, or any other piece of information that the system relies on.

When the DRY principle is applied successfully, a modification of any single element of a system does not require a change in other logically unrelated elements.

---

## Key Concepts

The core idea behind DRY is to avoid having the same piece of information or logic in multiple places. Repetition can lead to inconsistencies and maintenance nightmares. If you have the same code in two different places and you need to make a change, you have to remember to change it in both places. If you forget one, you've introduced a bug.

---

## Benefits of DRY

- **Improved Maintainability:** When a piece of logic is in only one place, it's easier to maintain and update.
- **Reduced Bugs:** Less code means fewer places for bugs to hide. It also eliminates the risk of inconsistencies that arise from duplicated logic.
- **Increased Readability:** Code is easier to read and understand when it's not cluttered with repetitive logic.
- **Enhanced Reusability:** By abstracting common logic, you create reusable components that can be used elsewhere.
- **Cost and Time Savings:** Less code to write and maintain saves time and money.

---

## Examples

### Violation of DRY

Consider an e-commerce application where the logic for calculating the total price of an order is duplicated in the shopping cart and at the final checkout.

```javascript
// In the shopping cart
function calculateCartTotal(items) {
    let total = 0;
    for (const item of items) {
        total += item.price * item.quantity;
    }
    // Apply a 10% discount for orders over $100
    if (total > 100) {
        total *= 0.9;
    }
    return total;
}

// At the checkout
function calculateCheckoutTotal(items) {
    let total = 0;
    for (const item of items) {
        total += item.price * item.quantity;
    }
    // Apply a 10% discount for orders over $100
    if (total > 100) {
        total *= 0.9;
    }
    return total;
}
```
If the discount logic changes (e.g., the discount becomes 15%), it needs to be updated in both functions. Forgetting to update one would lead to inconsistent pricing for the user.

### Application of DRY

The repeated logic can be extracted into a single function.

```javascript
// A single source of truth for price calculation
function calculateOrderTotal(items) {
    let total = 0;
    for (const item of items) {
        total += item.price * item.quantity;
    }
    // Apply a 10% discount for orders over $100
    if (total > 100) {
        total *= 0.9;
    }
    return total;
}

// In the shopping cart
function getCartTotal(items) {
    return calculateOrderTotal(items);
}

// At the checkout
function getCheckoutTotal(items) {
    return calculateOrderTotal(items);
}
```
Now, any change to the pricing logic only needs to be made in one place.

---

## Relationship with Other Principles

- **[[soc|Separation of Concerns (SoC)]]:** DRY and SoC are complementary. SoC is about breaking a system into distinct sections, each addressing a concern. DRY is about avoiding repetition of knowledge within the entire system. By separating concerns, you often reduce repetition.
- **Single Source of Truth (SSOT):** DRY is a direct implementation of the SSOT principle. The goal is to have one and only one place where a particular piece of knowledge is defined.

---

## Resources & links

These resources offer further insights into the DRY principle and how to apply it effectively in your projects.

### Articles

1.  **[Don't Repeat Yourself (DRY) in Software Development - GeeksforGeeks](https://www.geeksforgeeks.org/software-engineering/dont-repeat-yourselfdry-in-software-development/)**
    This article provides a clear definition of the DRY principle, explaining its importance in reducing redundancy and improving code maintainability with practical examples.

2.  **[The DRY Principle in Software Design - Baeldung](https://www.baeldung.com/cs/dry-software-design-principle)**
    Baeldung's guide explores the nuances of the DRY principle, discussing when and how to apply it, as well as potential pitfalls like premature abstraction.

### Videos

1.  **[The DRY Programming Principle](https://www.youtube.com/watch?v=8hOZe5oVzJY)**
    This video discusses the DRY (Don't Repeat Yourself) programming principle, which is frequently disregarded and misunderstood by junior developers. It includes examples related to constants and different business domains.

2.  **[What is DRY code?](https://www.youtube.com/watch?v=HwTcjWtDAfc)**
    This video explains that DRY stands for "Don't Repeat Yourself," a software engineering principle from "The Pragmatic Programmer" that advocates for a single, authoritative source of truth for every piece of knowledge in a system to reduce duplication and improve maintainability. The video also discusses the tradeoffs and potential misapplications of DRY.