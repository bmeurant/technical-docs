---
title: Composition Over Inheritance
tags:
  - design-principle
  - composition
  - inheritance
  - object-oriented
  - solid
date: 2025-10-06
---
# Composition Over Inheritance

**Composition Over Inheritance** is a principle in [[object-oriented-programming]] that states that classes should achieve polymorphic behavior and code reuse by their composition (containing instances of other classes) rather than inheritance from a base class.

In essence, it answers the question of how to reuse functionality. While inheritance provides an "is-a" relationship, composition provides a "has-a" relationship. The principle suggests that a "has-a" relationship is often more flexible and robust.

---

## Key Concepts: Inheritance vs. Composition

### The Problem with Inheritance

Inheritance is a powerful tool, but it can lead to several problems:

-   **Tight [[cohesion-coupling|Coupling]]:** A subclass is tightly coupled to its superclass. A change in the superclass can break the subclass in unexpected ways (the "fragile base class" problem).
-   **Rigid Hierarchies:** Inheritance hierarchies are defined at compile-time. This makes it difficult to change the behavior of an object at runtime.
-   **The "Gorilla-Banana" Problem:** As famously stated by Joe Armstrong, "You wanted a banana, but what you got was a gorilla holding the banana and the entire jungle." With inheritance, you often get more than you need.
-   **[[solid|Liskov Substitution Principle]] Violations:** It's easy to create inheritance hierarchies that violate the LSP, where a subclass cannot be used interchangeably with its superclass.

### The Power of Composition

Composition involves creating complex objects by combining simpler ones. Instead of inheriting behavior, an object delegates responsibility to other objects it contains.

-   **Flexibility:** Behavior can be changed at runtime by swapping out composed objects with different ones. This is a key tenet of the [[gof|Strategy Pattern]].
-   **Loose [[cohesion-coupling|Coupling]]:** The containing object only depends on the public interface of the composed object, not its implementation.
-   **[[object-oriented-programming#1-encapsulation|Encapsulation]]:** The internal details of the composed objects are hidden from the outside world.

---

## Code-Level Illustration

Let's consider a classic example in a game: different types of characters.

### Violation: A Rigid Inheritance Hierarchy

A naive approach might be to create a class hierarchy for different character abilities.

```javascript
class Character {
    attack() {
        console.log("Character attacks!");
    }
}

class Mage extends Character {
    castSpell() {
        console.log("Casting a powerful spell!");
    }
}

class Warrior extends Character {
    swingSword() {
        console.log("Swinging a heavy sword!");
    }
}

// What if we want a "Battlemage" that can both attack and cast spells?
// Multiple inheritance is not supported in JavaScript.
// We could create a Battlemage class that inherits from Mage, but what if we also want a Warrior that can use some magic?
// The hierarchy quickly becomes a mess.
```

### Application: A Flexible Composition Approach

With composition, we define behaviors (or "abilities") as separate objects and compose a character with the abilities it needs.

```javascript
// Define behaviors as separate components (Strategies)
const Attacker = {
    attack() {
        console.log("Character attacks!");
    }
};

const SpellCaster = {
    castSpell() {
        console.log("Casting a powerful spell!");
    }
};

const SwordSwinger = {
    swingSword() {
        console.log("Swinging a heavy sword!");
    }
};

// Create a Character factory that can be composed with different abilities
function createCharacter(name, abilities) {
    const character = { name };
    return Object.assign(character, ...abilities);
}

// Now we can create any type of character by composing behaviors
const mage = createCharacter("Gandalf", [Attacker, SpellCaster]);
mage.castSpell(); // "Casting a powerful spell!"

const warrior = createCharacter("Aragorn", [Attacker, SwordSwinger]);
warrior.swingSword(); // "Swinging a heavy sword!"

// A Battlemage is just another composition
const battlemage = createCharacter("Elara", [Attacker, SpellCaster, SwordSwinger]);
battlemage.attack();
battlemage.castSpell();
battlemage.swingSword();
```
This approach is far more flexible. We can easily add new abilities (e.g., `Flyer`, `Healer`) and combine them in any way we want without changing existing code, adhering to the **[[solid|Open/Closed Principle]]**.

---

## Relationship with Other Principles

-   **[[solid|SOLID Principles]]:** This principle is a practical application of several SOLID principles.
    -   It helps enforce the **Open/Closed Principle** by allowing new behaviors to be added without modifying existing classes.
    -   It helps prevent violations of the **Liskov Substitution Principle** that often occur with deep inheritance hierarchies.
    -   It aligns with the **Dependency Inversion Principle**, as high-level classes can depend on abstract behaviors rather than concrete implementations.
-   **[[gof|Design Patterns]]:** Many of the Gang of Four design patterns are based on this principle. The **Strategy**, **Decorator**, **Bridge**, and **Composite** patterns are prime examples of favoring composition.
-   **[[posa|Pattern-Oriented Software Architecture (POSA)]]**: The patterns in POSA often rely on composition to build flexible and distributed systems. For example, patterns for distributed systems often use composition to assemble communication and processing components. The **Whole-Part** pattern is a direct application of composition.

---

## Resources & links

### Articles

1.  **[Favor Composition Over Inheritance: Insights from Go](https://syntactic-sugar.dev/blog/nested-route/composition-over-inheritance)**
    This article explains how Go's design philosophy, which lacks traditional inheritance, naturally encourages composition, leading to more flexible and maintainable code.

2.  **[This is why you need Composition over Inheritance](https://theburningmonk.com/2015/03/this-is-why-you-need-composition-over-inheritance/)**
    A practical article that uses a clear example to demonstrate how inheritance can lead to rigid designs and how composition can solve the same problem with greater flexibility.

### Videos

1.  **[The Flaws of Inheritance](https://www.youtube.com/watch?v=hxGOiiR9ZKg)**
    This video provides a concise and clear explanation of the problems with inheritance, such as the fragile base class problem and tight coupling.

2.  **[Composition over Inheritance](https://www.youtube.com/watch?v=wfMtDGfHWpA)**
    A detailed video that explains the "is-a" vs. "has-a" relationship and why composition often leads to more flexible and reusable code.
