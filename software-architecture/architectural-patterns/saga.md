---
title: Saga Pattern
tags:
  - data-management
  - communication
date: 2025-09-27
---
# Saga Pattern

The **Saga** pattern is an [[software-architecture/architectural-patterns/|architectural pattern]] for managing data consistency in a distributed [[microservices|microservices architecture]]. It provides a way to handle long-lived transactions (LLTs) that span multiple services without relying on traditional two-phase commit (2PC) protocols, which are often impractical in a distributed environment due to their locking nature. A saga is a sequence of local transactions where each transaction updates the database within a single service and publishes an event or message that triggers the next transaction in the saga.

* **Core Principles:**
    *   **Sequence of Local Transactions:** A saga is composed of a series of atomic, local transactions, where each one is executed by a different service.
    *   **Compensating Transactions:** If any local transaction fails, the saga must roll back the preceding transactions. This is achieved by executing a corresponding **compensating transaction** for each completed step, in reverse order. A compensating transaction is an operation that undoes the effect of a previous transaction (e.g., a `CancelOrder` action to compensate for a `CreateOrder` action).
    *   **Atomicity (All or Nothing):** The saga guarantees that either all local transactions in the sequence complete successfully, or the system state is returned to its initial condition through compensation.

---

## Key Components and Communication Flow

There are two main ways to coordinate a saga: **Choreography** and **Orchestration**.

### 1. Choreography-Based Saga

In this approach, there is no central coordinator. Each service participating in the saga subscribes to events from other services and acts accordingly.

```mermaid
sequenceDiagram
    participant OrderService as Order Service
    participant PaymentService as Payment Service
    participant InventoryService as Inventory Service

    OrderService->>OrderService: 1. Create Order (Pending)
    OrderService-->>PaymentService: 2. Publish OrderCreated Event
    
    PaymentService->>PaymentService: 3. Process Payment
    alt Payment Successful
        PaymentService-->>OrderService: 4a. Publish PaymentProcessed Event
        OrderService->>OrderService: 5a. Mark Order as Paid
    else Payment Failed
        PaymentService-->>OrderService: 4b. Publish PaymentFailed Event
        OrderService->>OrderService: 5b. Execute Compensating Tx (Cancel Order)
    end

    InventoryService-->>OrderService: (Listens for PaymentProcessed)
```

*   **Pros:** Simple, decoupled, no single point of failure.
*   **Cons:** Can be hard to track the state of a transaction, risk of cyclic dependencies between services.

### 2. Orchestration-Based Saga

In this approach, a central **orchestrator** (or coordinator) is responsible for telling the saga participants what to do. The orchestrator manages the sequence of transactions and compensating transactions.

```mermaid
sequenceDiagram
    participant Orchestrator as Saga Orchestrator
    participant OrderService as Order Service
    participant PaymentService as Payment Service
    participant InventoryService as Inventory Service

    Orchestrator->>OrderService: 1. Execute CreateOrder Tx
    OrderService-->>Orchestrator: 2. Order Created
    
    Orchestrator->>PaymentService: 3. Execute ProcessPayment Tx
    alt Payment Successful
        PaymentService-->>Orchestrator: 4a. Payment Processed
        Orchestrator->>InventoryService: 5a. Execute UpdateInventory Tx
        InventoryService-->>Orchestrator: 6a. Inventory Updated
        Orchestrator->>Orchestrator: 7a. Saga Complete
    else Payment Failed
        PaymentService-->>Orchestrator: 4b. Payment Failed
        Orchestrator->>OrderService: 5b. Execute Compensating Tx (Cancel Order)
        OrderService-->>Orchestrator: 6b. Order Cancelled
        Orchestrator->>Orchestrator: 7b. Saga Rolled Back
    end
```

*   **Pros:** Centralized logic, easier to manage complexity, easier to debug and test.
*   **Cons:** Introduces a single point of failure (the orchestrator), services can be tightly coupled to the orchestrator.

---

## Advantages and Technical Challenges

* **Advantages (Benefits):**
    *   **Maintains Data Consistency:** Ensures data consistency across multiple services without using distributed transactions.
    *   **Improved Resilience:** Avoids the need for distributed locks, which improves system resilience and performance. Services are less coupled and can be scaled independently.
    *   **Flexibility:** Allows for long-running transactions that can last for hours or even days.

* **Challenges:**
    *   **Complexity:** Implementing sagas is complex. Developers need to design compensating transactions carefully.
    *   **Eventual Consistency:** The data is only consistent at the end of the saga. This can be challenging for developers and users who are used to immediate consistency.
    *   **Debugging:** Debugging a distributed transaction that spans multiple services can be difficult. Centralized logging and tracing are essential.
    *   **Lack of Isolation:** Unlike traditional ACID transactions, sagas do not have isolation. This means that other transactions can see the intermediate state of the saga. This needs to be handled carefully (e.g., by marking orders as "Pending").

---

## Related Patterns, Concepts and Variations

*   **[[microservices|Microservices]]:** The Saga pattern is a common solution for managing distributed transactions in a microservices architecture.
*   **[[event-sourcing|Event Sourcing]]:** Sagas can be implemented using events. In an event-sourced system, a saga can be seen as a process that reacts to events and produces new events.
*   **Transactional Outbox:** This pattern can be used to ensure that events are published reliably after a local transaction is committed. This is crucial for choreography-based sagas.
*   **Two-Phase Commit (2PC):** The Saga pattern is an alternative to 2PC. While 2PC provides strong consistency, it is often not suitable for distributed systems due to its blocking nature.

---

## **Resources & links**

### **Articles**

1.  **[Saga Pattern by Microsoft](https://docs.microsoft.com/en-us/azure/architecture/reference-architectures/saga/saga)**

    A comprehensive guide from Microsoft that explains the pattern in detail, with a focus on the orchestration and choreography approaches.

2.  **[Pattern: Saga by Chris Richardson](https://microservices.io/patterns/data/saga.html)**

    An excellent article by the author of "Microservices Patterns". It provides a clear explanation of the pattern and its trade-offs.

### **Videos**

1. **[The Saga Pattern in Microservices (EDA - part 2)](http://www.youtube.com/watch?v=C0rGwyJkDTU)** (A Dev' Story)

    Explains the **Saga Pattern** for managing **distributed transactions** across multiple **Microservices** that each have their own database. Details the two main implementation methods: **Orchestration** (centralized service controls the flow) and **Choreography** (services communicate via **Events**) with compensation logic for failures.

2. **[Saga Pattern For Microservice Architecture With Rebus And RabbitMQ](http://www.youtube.com/watch?v=dlXCodLxhag)** (Milan Jovanović)

    A practical deep dive showing how to implement the **Orchestrated Saga Pattern** in a .NET Core microservice environment. Uses the **Rebus** library and **RabbitMQ** as the message broker, with PostgreSQL for persistence, to ensure data consistency through a defined sequence of steps and handlers.
