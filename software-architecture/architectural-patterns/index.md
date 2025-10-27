---
title: Architectural Patterns
tags:
  - architectural-pattern
date: 2025-09-19
---

# Architecture Patterns

An **architectural pattern** is a reusable, concrete solution to a recurring design problem, often applicable to the internal organization of modules or services. While a style is a blueprint for the entire house, a pattern is a blueprint for a specific room or system inside that house, like the plumbing. Typical examples include **[[hexagonal|Hexagonal Architecture]]**, which organizes business logic by isolating it from external dependencies, the [[mvc|Model-View-Controller (MVC)]] pattern, which organizes user interface logic, or the [[event-sourcing|Event Sourcing]] pattern, which manages data persistence. The commonality with styles, as explained on the "[[software-architecture/architectural-styles/|Architectural Styles]]" page, is that both provide proven solutions to design problems. The key difference lies in their scope: patterns define an organization at the scale of a subsystem or module, **without dictating the global structure of the entire system**, while styles define the organization of the entire ecosystem.

## Application Architecture Patterns

*   [[blackboard|Blackboard Pattern]]
*   [[broker|Broker]]
*   [[clean|Clean Architecture]]
*   [[cqrs|CQRS (Command Query Responsibility Segregation)]]
*   [[event-sourcing|Event Sourcing]]
*   [[hexagonal|Hexagonal Architecture]]
*   [[modern-application-architectures|Modern Application Architectures]]
*   [[modular-monolith|Modular Monolith]]
*   [[mvc|Model-View-Controller (MVC)]]
*   [[mvp|Model-View-Presenter (MVP)]]
*   [[mvvm|Model-View-ViewModel (MVVM)]]
*   [[mvw|Model-View-Whatever (MVW)]]
*   [[onion|Onion Architecture]]
*   [[pipe-filters|Pipe and Filters Pattern]]
*   [[saga|Saga]]
*   [[service-mesh|Service Mesh]]
*   [[strangler-fig|Strangler Fig]]
*   [[api-gateway|API Gateway]]

## Links and Resources

#### Videos

1. **[Technical Architecture Styles vs Patterns Explained in Simple Terms](https://www.youtube.com/watch?v=5FbDO8bHEko)**