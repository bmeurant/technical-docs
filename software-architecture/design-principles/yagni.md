---
title: You Aren't Gonna Need It (YAGNI)
tags:
  - design-principle
  - agile
  - simplicity
date: 2025-10-02
---
# You Aren't Gonna Need It (YAGNI)

**You Aren't Gonna Need It (YAGNI)** is a principle of extreme programming (XP) that states a programmer should not add functionality until it is deemed necessary. YAGNI is a practice of simplicity and minimalism in software development.

The principle is often summarized as "Always implement things when you actually need them, never when you just foresee that you need them."

---

## Key Concepts

YAGNI is about resisting the temptation to build features that are not immediately required. It's a common tendency for developers to anticipate future needs and build generic solutions or add extra features just in case they might be useful later. YAGNI advises against this, as it often leads to code that is more complex, harder to maintain, and may never even be used.

The core idea is to focus on the current requirements and deliver the simplest possible solution that works.

---

## Benefits of YAGNI

- **Reduced Complexity:** The codebase remains as simple as possible, making it easier to understand and maintain.
- **Faster Development:** By focusing only on the necessary features, development time is reduced.
- **Lower Costs:** Less code means less time and money spent on development, testing, and maintenance.
- **Higher Quality:** A simpler codebase is less likely to contain bugs.
- **Flexibility:** It's easier to change a simple system than a complex one. YAGNI prevents the team from being locked into a design that may not be suitable for future requirements.

---

## Examples

### Violation of YAGNI

Imagine a developer is building a blog application. The current requirement is to allow users to have a single role: "author". The developer might think, "What if we need more roles in the future, like 'editor' or 'admin'?" and then build a complex role management system with permissions and inheritance.

This is a violation of YAGNI because the extra complexity is not needed right now. The time spent building this complex system could have been spent on more pressing features.

### Application of YAGNI

A developer following YAGNI would simply add a `role` field to the `User` model with the value "author". If, in the future, more roles are needed, the system can be refactored at that time. This approach is much faster and results in a simpler system.

---

## Code-Level Illustration

Let's contrast this with a code example.

### Violation of YAGNI (Anticipating Future Needs)

A developer is asked to create a function that returns a greeting. The only requirement is for it to greet in English. Fearing that more languages will be requested later, they build a more extensible solution upfront.

```javascript
function getGreeting(language = 'en') {
    switch (language) {
        case 'en':
            return 'Hello, World!';
        case 'es':
            return '¡Hola, Mundo!';
        // A developer might even leave placeholders for future work
        // case 'fr':
        //     return 'Bonjour, le monde!';
        default:
            return 'Hello, World!';
    }
}

// The feature was built, but only the 'en' case is actually used.
console.log(getGreeting('en'));
```
The problem here is that time and effort were spent implementing and testing the Spanish case (`'es'`) which was never requested. This is a classic YAGNI violation.

### Application of YAGNI (Implementing Only What's Necessary)

Following YAGNI, the developer implements only the feature that was explicitly asked for.

```javascript
function getGreeting() {
    return 'Hello, World!';
}

console.log(getGreeting());
```
This code is not only simpler (adhering to KISS), but it perfectly matches the current requirements. It took less time to write, is easier to test, and fully delivers the required value. If Spanish support is needed later, it can be added *at that time*.

---

## Relationship with Other Principles

- **[[dry|Don't Repeat Yourself (DRY)]]:** YAGNI and DRY are complementary. While DRY is about avoiding duplication, YAGNI is about avoiding unnecessary work. Both principles lead to simpler, more maintainable code.
- **[[kiss|KISS (Keep It Simple, Stupid)]]:** YAGNI is a direct and practical application of the KISS principle. While KISS is a general philosophy to avoid unnecessary complexity in all its forms, YAGNI provides a clear rule to achieve that simplicity: do not implement features that are not currently required. Every time you successfully apply YAGNI, you are also adhering to KISS because you are preventing the introduction of code that would make the system more complex. They are complementary: YAGNI addresses *what* to build (or rather, what *not* to build), while KISS addresses *how* to build it (simply). Following YAGNI is one of the most effective ways to maintain a simple system.

---

## Resources & links

### Articles

1.  **[Yagni by Martin Fowler](https://martinfowler.com/bliki/Yagni.html)**
    Martin Fowler discusses the "You Aren't Gonna Need It" principle from Extreme Programming, which advocates against building features for future presumptive needs.

2.  **[YAGNI Principle in Software Development - GeeksforGeeks](https://www.geeksforgeeks.org/software-engineering/what-is-yagni-principle-you-arent-gonna-need-it/)**
    This article explains that YAGNI, or "You Aren't Gonna Need It," is a software development principle advocating for implementing only currently necessary features to avoid increased complexity, longer development times, and more bugs.

### Videos

1.  **[The YAGNI Principle - Venkat Subramaniam](https://www.youtube.com/watch?v=QyOQBuHDAoY)**
    Venkat Subramaniam discusses the YAGNI (You Aren't Gonna Need It) principle, presenting it as a lesser-known counterpart to the DRY (Don't Repeat Yourself) principle.

2.  **[Do you really need that abstraction or generic code? (YAGNI)](https://www.youtube.com/watch?v=_Al7qI4vMt0)**
    This video discusses the "You Aren't Gonna Need It" (YAGNI) principle, advising against developing features or code today that are only assumed to be needed in the future.
