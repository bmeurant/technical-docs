--- 
title: The Hollywood Principle (IoC)
tags:
  - design-principle
  - inversion-of-control
  - framework
  - coupling
  - dependency-inversion
date: 2025-10-06
---
# The Hollywood Principle (Inversion of Control)

The **Hollywood Principle** is a design principle that inverts the flow of control in a software system. It gets its name from the classic Hollywood movie industry refrain:

> "Don't call us, we'll call you."

In software, this means that high-level components or frameworks should call low-level components, not the other way around. The low-level components (your custom code) don't get to decide when they are called; they are called by the framework at the appropriate time.

This principle is the foundation of **Inversion of Control (IoC)**, a hallmark of modern software frameworks (like Spring, Angular, or ASP.NET).

---

## Core Idea: Inverting the Flow of Control

In traditional, procedural programming, your code is in charge. It controls the flow of execution and calls library functions when it needs them. The main program is the boss.

```
// Traditional Control Flow
main() {
    while(true) {
        String input = readUserInput();
        Result result = process(input);
        print(result);
    }
}
```

With the Hollywood Principle, this is inverted. You write reusable components or handlers, and you plug them into a framework. The **framework is the boss**. It runs the main loop and calls your code when a specific event occurs.

```
// Inverted Control Flow (Hollywood Principle)
class MyButtonHandler implements ClickHandler {
    // The framework calls this method when the button is clicked.
    onClick(event) {
        // Your custom logic here
    }
}

// You register your handler with the framework.
framework.registerClickHandler(myButton, new MyButtonHandler());
// The framework's main event loop runs and will call you.
framework.run();
```

Your code doesn't call the framework's event loop; the framework calls your code.

---

## Code Examples

### Violation: Traditional Control

Imagine a system where a high-level `ReportGenerator` class depends directly on and controls a low-level `DatabaseReader`.

```javascript
// The high-level component is in control.
class ReportGenerator {
    generateReport() {
        const reader = new DatabaseReader();
        const data = reader.readData(); // The generator directly calls the reader.
        console.log("Generating report with data:", data);
    }
}

class DatabaseReader {
    readData() {
        return "Data from DB";
    }
}

// The application code calls the high-level component.
const generator = new ReportGenerator();
generator.generateReport();
```

**Why is this a problem?** The `ReportGenerator` is tightly coupled to the `DatabaseReader`. If you want to generate a report from a file or an API, you have to modify the `ReportGenerator` class.

### Application: Inversion of Control

With the Hollywood Principle, the framework (or a generic orchestrator) is responsible for providing the dependency. The high-level component simply declares what it needs.

```javascript
// The high-level component just exposes a dependency.
class ReportGenerator {
    constructor(dataReader) {
        // The dependency is "injected" by an external entity.
        this.dataReader = dataReader;
    }

    generateReport() {
        // It uses the dependency it was given.
        const data = this.dataReader.readData();
        console.log("Generating report with data:", data);
    }
}

// Low-level components conform to a common interface.
class DatabaseReader {
    readData() { return "Data from DB"; }
}

class FileReader {
    readData() { return "Data from File"; }
}

// The framework or "main" function wires everything up.
// It controls the creation and calls our code.
const dbReader = new DatabaseReader();
const generator = new ReportGenerator(dbReader); // The framework calls the constructor.
generator.generateReport(); // The framework calls the action method.
```
Here, the control of creating and providing the `DatabaseReader` has been inverted. It is now handled by an external entity, not the `ReportGenerator` itself. This is the essence of IoC.

---

## Benefits

-   **[[cohesion-coupling|Loose Coupling]]:** Components no longer depend on the concrete implementations of other components, only on their interfaces. This makes the system much more flexible.
-   **Extensibility & Pluggability:** It is easy to add new functionality by creating new low-level components that can be "plugged into" the framework without changing the high-level components.
-   **[[soc|Separation of Concerns]]:** The framework handles the generic, boilerplate logic (like event loops, request routing, or dependency creation), allowing your components to focus solely on their specific business logic.

---

## Relationship with Other Principles

-   **[[solid|Dependency Inversion Principle (DIP)]]:** The Hollywood Principle is a broader, architectural application of DIP. Both principles aim to invert dependencies. DIP states that high-level modules should not depend on low-level modules but on abstractions. The Hollywood Principle takes this further by inverting the entire flow of control, where a framework calls into the application code.
-   **Dependency Injection (DI):** DI is a common pattern used to implement the Hollywood Principle. The framework (or an "IoC Container") injects dependencies into a component, rather than letting the component create them itself.
-   **[[gof|Template Method & Strategy Patterns]]:** Frameworks often use these patterns to implement IoC. The framework defines the high-level algorithm (the "template method"), and the custom code provides the implementation for specific, variable steps (the "strategy").
-   **[[event-driven|Event-Driven Architecture]]:** This principle is fundamental to event-driven systems. Components don't call each other directly; they react to events that are dispatched by a central framework or event bus. They don't call the event bus; the event bus calls them.
-   **Architectural Styles:** The principle is a key enabler for styles like [[layered|Layered Architecture]] and [[modular-monolith|Modular Monoliths]], where frameworks manage dependencies between layers or modules to enforce boundaries and reduce coupling.

---

## Resources & Links

These resources will provide you with a solid foundation to master the Hollywood principle and apply its concepts in your code.

### Articles

1.  **[Inversion Of Control](https://martinfowler.com/bliki/InversionOfControl.html)**

    This is a foundational article by Martin Fowler that clearly defines Inversion of Control and distinguishes it from Dependency Injection. It's a must-read to understand the roots of the Hollywood Principle.

2.  **[Hollywood Principle in Software Design](https://medium.com/@sandy619g/hollywood-principle-in-software-engineering-5d68f679b524)**

    This article provides a straightforward explanation of the Hollywood Principle with practical code examples, making it easy to grasp how it helps in building decoupled and maintainable systems.

### Videos

1.  **[Dependency Injection & Inversion of Control](https://www.youtube.com/watch?v=EPv9-cHEmQw)**

    This video offers a clear visual explanation of Inversion of Control and its relationship with Dependency Injection, helping to solidify the concepts.

2.  **[Inversion of Control, simplified](https.www.youtube.com/watch?v=5r8Byu2KRuU)**

    A concise and simplified explanation of IoC, perfect for getting a quick and clear understanding of this fundamental design principle.
