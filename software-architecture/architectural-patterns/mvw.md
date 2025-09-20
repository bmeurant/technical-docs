---
title: Model-View-Whatever (MVW)
---
# Model-View-Whatever (MVW)

The term **MVW**, or **Model-View-Whatever**, isn't a strict [[software-architecture/architectural-patterns/|architectural pattern]] but a generic concept. It's used to describe a family of design patterns, including [[mvc|MVC]], [[mvp|MVP]], and **MVVM**, that share the fundamental principle of **separation of concerns**. They divide application logic into three distinct layers: the **Model**, the **View**, and an intermediary that handles presentation logic and communication.

The "Whatever" highlights the flexibility of this approach. The main goal isn't to follow a specific pattern name but to apply sound design principles to create maintainable, reusable, and easily testable code. This philosophy is especially relevant in the modern web framework ecosystem, where each technology may have its own implementation.

---

## Key "Model-View" Patterns

### 1. [[mvc|MVC (Model-View-Controller)]]
**[[mvc|MVC]]** is the predecessor to many other patterns. It separates the application into three key components:
* **Model**: Manages the application's data and business logic. It is independent of the user interface and notifies the **View** of any changes.
* **View**: Represents the user interface. It observes the **Model** and updates itself accordingly. It can also capture user actions and send them to the **Controller**.
* **Controller**: Acts as an "action router." It receives user requests from the **View**, processes them, updates the **Model**, and/or decides which **View** to display.

### 2. [[mvp|MVP (Model-View-Presenter)]]
**[[mvp|MVP]]** is an alternative to [[mvc|MVC]], often used in desktop and mobile applications. It aims to make presentation logic more easily testable by creating a passive **View**.
* **Model**: Similar to [[mvc|MVC]], it contains the data and business logic.
* **View**: The user interface. It is considered "passive" because it only displays what the **Presenter** tells it to and delegates all user actions to the **Presenter**. It contains no presentation logic.
* **Presenter**: Acts as the link between the **Model** and the **View**. It holds all the presentation logic, retrieves data from the **Model**, formats it, and sends it to the **View** for display. It has a direct dependency on the **View** via an interface.

### 3. MVVM (Model-View-ViewModel)
**MVVM** is a very popular pattern in modern web frameworks. It's designed to simplify **two-way data binding**.
* **Model**: The data and business logic, as in the other patterns.
* **View**: The user interface. It is **declarative** and is bound to the **ViewModel** via a binding mechanism. It doesn't know about the **Model** directly.
* **ViewModel**: The intermediary layer that exposes the **Model**'s data in a way that the **View** can easily bind to it. Unlike the **Presenter**, the **ViewModel** has no reference to the **View**, making it highly testable. Communication between the **View** and the **ViewModel** happens through a binding system and commands.

---

## Detailed Comparison ([[mvc|MVC]] vs. [[mvp|MVP]] vs. [[mvvm|MVVM]])

Here is a comparison table to better understand the nuances between these three patterns.

| Characteristic | MVC (Model-View-Controller) | MVP (Model-View-Presenter) | MVVM (Model-View-ViewModel) |
| :--- | :--- | :--- | :--- |
| **View/Intermediary Relationship** | The **View** has no direct reference to the **Controller**. The **Controller** does not directly manipulate the **View**. | The **View** has a reference to the **Presenter** (via an interface). The **Presenter** has a direct reference to the **View**. | The **View** has a reference to the **ViewModel**. The **ViewModel** has **no knowledge** of the **View**. |
| **View's Role** | **Active**. It can update itself by observing the **Model**. | **Passive**. It only displays what the **Presenter** tells it to. | **Declarative**. It updates automatically via `data binding` with the **ViewModel**. |
| **Intermediary's Role** | **Action router**. It handles user interactions and updates the **Model**. | **Direct intermediary**. It contains the presentation logic and manually updates the **View**. | **Model for the View**. It exposes the necessary data and `commands` for the **View** via binding. |
| **Communication** | The **Controller** receives events, updates the **Model**. The **View** updates by observing the **Model**. | The **View** notifies the **Presenter** of events. The **Presenter** updates the **View** and the **Model**. | The **View** and the **ViewModel** are synchronized via a **data binding** mechanism. |

## Conclusion

**MVW** represents a pragmatic and flexible approach to software architecture. The choice between [[mvc|MVC]], [[mvp|MVP]], and [[mvvm|MVVM]] depends on the technology, project constraints, and user interface complexity. While [[mvc|MVC]] is ideal for traditional server-side architectures, [[mvp|MVP]] is often preferred for applications where testability is critical, and MVVM is the go-to for modern frameworks that leverage `data binding`, offering excellent separation and ease of maintenance.

---

## **Resources & links**

### **Articles**

1.  **[MVC vs MVP vs MVVM](https://www.bacancytechnology.com/blog/mvc-vs-mvp-vs-mvvm)**

    This **Bacancy Technology** article compares the [[mvc|MVC]], [[mvp|MVP]], and [[mvvm|MVVM]] architectural patterns. It explains their role in managing complex code and improving UI code cleanliness, analyzing their performance, testability, and ease of modification, particularly in the context of Android development.

2.  **[Architecture Patterns for Beginners: MVC, MVP, and MVVM](https://dev.to/chiragagg5k/architecture-patterns-for-beginners-mvc-mvp-and-mvvm-2pe7)**

    This article on **dev.to** is aimed at beginners and introduces the [[mvc|MVC]], [[mvp|MVP]], and [[mvvm|MVVM]] architectural patterns. It defines the common components (**Model**, **View**) and then details the specific roles of the **Controller**, **Presenter**, and **ViewModel** for each pattern, offering a comparison of their key differences.

---

### **Videos**

1.  **[Exploring UI Design Patterns: MVC, MVP, and MVVM](https://www.youtube.com/watch?v=tm_paZsPsrI)**

    This video from **Andrey Tech** explores the [[mvc|MVC]], [[mvp|MVP]], and [[mvvm|MVVM]] UI design patterns, focusing on their application in mobile development. It explains how these patterns organize and optimize the logic of the presentation layer to improve code modularity and testability.

2.  **[Everything You NEED to Know About Client Architecture Patterns](https://www.youtube.com/watch?v=I5c7fBgvkNY)**

    This video from **ByteByteGo** dives into the world of client architectural patterns, covering the workings of [[mvc|MVC]], [[mvp|MVP]], and [[mvvm|MVVM]] through a practical example of a user updating their profile picture. It also compares more advanced patterns such as **MVVC** and **VIPER**.