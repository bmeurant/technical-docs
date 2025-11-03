---
title: KISS
tags:
  - design-principle
  - kiss
  - simplicity
  - yagni
date: 2025-10-03
---
# Keep It Simple, Stupid (KISS)

**Keep It Simple, Stupid (KISS)** is a design principle that originated in the U.S. Navy in 1960. The principle states that most systems work best if they are kept simple rather than made complicated; therefore, simplicity should be a key goal in design, and unnecessary complexity should be avoided.

The phrase is a reminder to combat the tendency of engineers to over-engineer solutions.

---

## Key Concepts

The core idea of KISS is that a simple solution is easier to understand, maintain, and debug than a complex one. This doesn't mean that the solution should be dumb or simplistic, but rather that it should be straightforward and clear.

Complexity can come in many forms: complex conditional logic, a large number of classes and methods, or the use of obscure language features. KISS encourages developers to favor clarity and simplicity over cleverness.

---

## Benefits of KISS

- **Improved Readability:** Simple code is easier for everyone on the team to read and understand.
- **Easier Maintenance:** It is easier to find and fix bugs in a simple system. It's also easier to add new features.
- **Higher Quality:** Simple systems have fewer interdependencies and fewer places for things to go wrong, which generally leads to higher quality and fewer bugs.
- **Faster Onboarding:** New team members can get up to speed more quickly on a simple codebase.

---

## Examples

### Violation of KISS (Over-engineering)

Imagine you need a function that returns a greeting in English. A developer might over-engineer the solution by using a complex factory pattern, even for this simple, single case.

```javascript
class Greeter {
    constructor(greeting) {
        this.greeting = greeting;
    }
    greet() {
        return this.greeting;
    }
}

class GreeterFactory {
    createGreeter(language) {
        // This switch and factory is overly complex for a single language.
        switch (language) {
            case 'en':
                return new Greeter('Hello, World!');
            default:
                return new Greeter('Hello, World!');
        }
    }
}

const factory = new GreeterFactory();
const greeter = factory.createGreeter('en');
console.log(greeter.greet());
```
This solution is unnecessarily complex. The use of classes and a factory pattern for a problem that can be solved with a single, simple function is a clear violation of KISS.

### Application of KISS (Simple and Direct)

A simpler approach is to write a straightforward function that does exactly what is needed.

```javascript
function getGreeting() {
    return 'Hello, World!';
}

console.log(getGreeting());
```
This code is much simpler, easier to read, and directly solves the problem. If support for other languages is needed in the future, the function can be refactored at that time.

---

## Relationship with Other Principles

- **[[yagni|You Aren't Gonna Need It (YAGNI)]]:** KISS and YAGNI are very closely related, but KISS is the broader principle. KISS advises that the simplest solution is the best. YAGNI is a specific and aggressive application of KISS that targets one of the biggest sources of complexity: speculative features. By strictly following YAGNI (not adding functionality until it's required), you are inherently making your system simpler and thus adhering to KISS. However, you can still violate KISS even if you are following YAGNI. For example, you could implement a *necessary* feature in an overly complex way (e.g., using an elaborate design pattern where a simple function would suffice). In short: YAGNI helps achieve KISS by reducing scope, while KISS is concerned with simplicity in both scope and implementation.
- **[[dry|Don't Repeat Yourself (DRY)]]:** While DRY helps to reduce redundancy, it can sometimes lead to complex abstractions if taken too far. KISS provides a balance by reminding us to keep those abstractions simple.

---

## Resources & links

These resources provide more depth on the KISS principle and its practical application in software engineering.

### Articles

1.  **[Understanding the “KISS” Principle in Software Design](https://medium.com/@Masoncoding/understanding-the-kiss-principle-in-software-design-keep-it-simple-stupid-6f5fcd8913f3)**
    This article advocates for simplicity to create maintainable, readable, and effective code, providing practical examples and warnings against overengineering.

2.  **[KISS Principle in Software Development - GeeksforGeeks](https://www.geeksforgeeks.org/software-engineering/kiss-principle-in-software-development/)**
    A guide that explains the importance of simplicity for clarity, efficiency, and risk reduction, and provides steps to apply the principle.

### Videos

1.  **[What is the KISS rule in programming?](https://www.youtube.com/watch?v=bEG94zyZlX0)**
    This video explains the origin of the "Keep it Simple, Stupid" principle and how it applies to software development through simpler abstractions and modular design.

2.  **[Why the K.I.S.S Principal is probably the most important](https://www.youtube.com/watch?v=Cml5Tb4PTbA)**
    This video explores the fundamental importance of the "Keep It Simple, Stupid" principle in the context of building robust and maintainable software systems.