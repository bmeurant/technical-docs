---
title: Modern Application Architectures
tags:
  - structure
---

# **Modern Application Architectures**

---

## **1. The Starting Point: The [[layered|Layered Architecture]]**

The **[[layered|Layered Architecture]]** is the most common and traditional architectural pattern. It organizes application components into horizontal layers, typically a **Presentation Layer** (UI, API), a **Business Logic Layer** (Services), and a **Data Access Layer** (Repositories, DAOs).

Dependencies flow **downwards**: the Presentation Layer depends on the Business Logic Layer, which depends on the Data Access Layer.

**The fundamental problem:** Business logic, the core of your application, is tightly coupled to the persistence technology. This makes the architecture **rigid** and **difficult to test** in isolation. To unit test your business logic, you often need a real database or a complex mocking setup. This violates the **[[solid|Dependency Inversion Principle (DIP)]]**, where high-level modules (business logic) should not depend on low-level modules (data access).

## **2. The Evolution: Inverting Dependencies**

To solve the limitations of the layered approach, modern architectures like **[[hexagonal|Hexagonal]]**, **[[onion|Onion]]**, and **Clean** all embrace the [[solid|Dependency Inversion Principle]]. Their core idea is to reverse the dependency flow. Instead of the business logic depending on the infrastructure, the infrastructure depends on interfaces defined by the business logic.

The dependency flow is always **inward**, creating a protective shield around the most important code—your business rules. This is the key difference that separates them from the [[layered|Layered Architecture]].

## **3. The Nuances: Comparing the Modern Architectures**

While sharing the same core principle, these three architectures differ in their level of detail, structure, and formalization.

### **[[hexagonal|Hexagonal Architecture]] (Ports and Adapters)**

* **Key Idea:** Focuses on the **boundary between the inside and outside**. The application's core logic is an isolated "hexagon" that communicates with the external world (UI, databases, external services) via **Ports** (interfaces) and **Adapters** (concrete implementations of those interfaces).
* **Focus:** It's a pragmatic principle. It emphasizes that external components are just "pluggable adapters" that can be swapped out easily. It doesn't provide strict rules on how to structure the code **inside** the hexagon.

### **[[onion|Onion Architecture]]**

* **Key Idea:** A more structured take on the hexagonal approach. It arranges the application in concentric rings, with the **Domain Model** at the very center. Dependencies only ever point inward.
* **Focus:** It is more prescriptive and places a strong emphasis on **Domain-Driven Design (DDD)**. It formalizes the layers around the core: `Domain Model`, `Domain Services`, `Application Services`, and `Infrastructure`.

### **[[clean|Clean Architecture]]**

* **Key Idea:** A synthesis of the best ideas from [[hexagonal|Hexagonal]], [[onion|Onion]], and other domain-centric architectures, popularized by Robert C. Martin ("Uncle Bob"). It is the most opinionated and well-defined of the three.
* **Focus:** It introduces specific names for its concentric circles and their responsibilities: `Entities`, `Use Cases`, `Interface Adapters`, and `Frameworks & Drivers`. It's highly prescriptive and formalizes the concept of `Use Cases` as the application's specific business logic.

---

### **Comprehensive Comparison**

| Characteristic | **[[layered|Layered Architecture]]** | **[[hexagonal|Hexagonal Architecture]]** | **[[onion|Onion Architecture]]** | **[[clean|Clean Architecture]]** |
| :--- | :--- | :--- | :--- | :--- |
| **Philosophy** | **Stacking** of layers. | **Isolation** via **Ports & Adapters**. | **Concentric** around the **Domain Model**. | **Synthesis** for independence. |
| **Origin** | Traditional | Alistair Cockburn (2005) | Jeffrey Palermo (2008) | Robert C. Martin (2012) |
| **Dependency Flow** | **Downward** (`UI` -> `Service` -> `Repo`). | **Inverted** (infrastructure depends on core). | **Inverted** (outer depends on inner). | **Inverted** (dependencies point inward). |
| **Coupling** | **High**. Business logic coupled to infrastructure. | **Low**. The core is decoupled. | **Low**. The domain is independent. | **Very Low**. Business logic is isolated. |
| **Key Components** | `UI`, `Business Logic`, `Data Access`. | `Core`, `Ports`, `Adapters`. | `Domain Model`, `Domain Services`, `Application Services`, `Infrastructure`. | `Entities`, `Use Cases`, `Interface Adapters`, `Frameworks & Drivers`. |
| **Testability** | **Difficult** in isolation. | **Excellent**. The core is testable in isolation. | **Excellent**. The domain is testable. | **Excellent**. `Use Cases` and `Entities` are easily unit-tested. |
| **Flexibility** | **Low**. Difficult to change technologies. | **High**. Allows changing technologies easily. | **High**. Facilitates technological changes. | **Very High**. The most robust and flexible. |
| **Ideal Use Case** | Small applications, simple services. | [[microservices|Microservices]], apps with multiple interfaces. | Projects with rich domain models (**DDD**). | Large-scale enterprise projects, critical services. |

---

## **Resources & links**

### **Articles**

1.  **[Understanding Hexagonal, Clean, Onion, and Traditional Layered Architectures: A Deep Dive](https://romanglushach.medium.com/understanding-hexagonal-clean-onion-and-traditional-layered-architectures-a-deep-dive-c0f93b8a1b96)**

    This article by **Roman Glushach** compares several architectural styles. It explains that **[[hexagonal|Hexagonal]]**, **Clean**, and **[[onion|Onion]]** architectures aim to solve the coupling issues of traditional layered architectures by placing the **business logic** at the core of the system. The article details how these approaches use the **[[solid|Dependency Inversion Principle]]** to ensure the application's core remains independent of external components like databases and user interfaces, thereby improving maintainability and testability.

2.  **[N-Tier vs Hexagonal vs Onion vs Clean Architecture in Very Simple Terms](https://medium.com/@dorinbaba/n-tier-vs-hexagonal-vs-onion-vs-clean-architecture-in-very-simple-terms-68f66c4dba22)**

    In this article, **Dorin Baba** provides a simplified overview of several architectural models. It compares the traditional **N-Tier** architecture to **[[hexagonal|Hexagonal]]** and its evolutions, **[[onion|Onion]]** and **Clean** architectures. The article highlights how these modern architectures place the **business logic** at the center and rely on **[[solid|Dependency Inversion]]** to separate concerns and prevent infrastructure decisions from dictating the application's structure.

3.  **[Hexagonal vs Clean vs Onion Architectures](https://programmingpulse.vercel.app/blog/hexagonal-vs-clean-vs-onion-architectures)**

    This article focuses on the similarities and differences between **[[hexagonal|Hexagonal]]**, **Clean**, and **[[onion|Onion]]** architectures. It emphasizes that these three models share common goals, such as **separation of concerns**, **testability**, and **modularity**. The article concludes that there is no one-size-fits-all solution, and the choice of architecture depends on the specific project needs, team skills, and the requirement for scalability and maintainability.

---

### **Videos**

1.  **[Layered, Hexagonal, Onion and Clean Architecture](https://www.youtube.com/watch?v=rBNb3xHJUZk)**

    This video by **Mo Pazooki** traces the evolution of software architectures from traditional layered designs to modern **[[hexagonal|Hexagonal]]**, **[[onion|Onion]]**, and **Clean** architectures. It explains how each architectural style solved the coupling and dependency issues of its predecessor by increasingly focusing on **business logic** and separating it from infrastructure and UI concerns. The video argues that these architectures aim to create more robust, flexible, and maintainable systems.

2.  **[Hexagonal, Onion, and Clean Architecture: A Deep Dive](https://www.youtube.com/watch?v=0X3_a5a7Jc0)**

    In this video, **Milan Jovanović** provides a detailed comparison of **[[hexagonal|Hexagonal]]**, **[[onion|Onion]]**, and **Clean** architectures. He explains that while they share the same core principle of **[[solid|Dependency Inversion]]**, they differ in their level of detail and structure. The video highlights that **[[hexagonal|Hexagonal]]** focuses on the boundary between the application and the outside world, **[[onion|Onion]]** adds more structure around the domain model, and **Clean** is the most prescriptive, with a strong emphasis on `Use Cases`.

3.  **[Clean vs Hexagonal vs Onion Architecture](https://www.youtube.com/watch?v=n4nFf8i8v1E)**

    This video by **Amichai Mantinband** compares **Clean**, **[[hexagonal|Hexagonal]]**, and **[[onion|Onion]]** architectures, highlighting their common goal of creating a **domain-centric** system where business logic is independent of external technologies. It explains how these architectures use **[[solid|Dependency Inversion]]** to ensure that dependencies flow inward, making the application more testable, maintainable, and flexible. The video concludes that the choice between them often comes down to team preference and the specific needs of the project.
