---
title: Publish-Subscribe Pattern
tags:
  - messaging
  - communication
---
# **Publish-Subscribe Pattern**

The **Publish-Subscribe** (or Pub/Sub) pattern is an [[event-driven|event-driven]] [[software-architecture/architectural-patterns/|architectural pattern]] where message senders, known as **publishers**, do not know the receivers, known as **subscribers**. Messages are categorized by **topics** or **channels**. The core of the system is an intermediary component, often called a **[[broker|message broker]]** or **event bus**, which manages message distribution.

---

## **Core Principles**

* **[[cohesion-coupling|Decoupling]]:** This is the key principle. Publishers and subscribers are completely independent of each other. A publisher does not know who (or how many) subscribers are listening to its messages, and vice versa. This temporal and spatial decoupling facilitates the independent development and evolution of services.
* **Asynchrony:** Communication is non-blocking. A publisher sends a message and does not expect an immediate response. The message is stored by the [[broker]] and delivered to the subscribers when they are available.
* **Topic-Based Messaging:** Messages are categorized by topics. Subscribers subscribe to one or more topics to receive the messages that interest them. This model allows for selective and efficient information broadcast.
* **Scalability:** It is very easy to add new publishers or subscribers without modifying existing components. The load can be distributed by adding new consumer instances.

---

## **Key Components and Communication Flow**

```mermaid
graph TD
    subgraph Publishers
        A[Service A - Publisher]
        B[Service B - Publisher]
    end

    subgraph [[broker|Message Broker]]
        C[Topic 1]
        D[Topic 2]
    end

    subgraph Subscribers
        E[Service X - Subscriber]
        F[Service Y - Subscriber]
        G[Service Z - Subscriber]
    end

    A -- "publish - Topic 1" --> C
    B -- "publish - Topic 2" --> D

    C -- "deliver" --> E
    C -- "deliver" --> F
    D -- "deliver" --> G

    style A fill:#BCEE68,stroke:#000
    style B fill:#BCEE68,stroke:#000
    style E fill:#ADD8E6,stroke:#000
    style F fill:#ADD8E6,stroke:#000
    style G fill:#ADD8E6,stroke:#000
```


1.  **Publisher:** The entity that sends a message (an event or a command) without knowing the recipients. It only publishes to a topic.
2.  **[[broker|Message Broker]]:** The central component that receives messages from publishers and routes them to the relevant subscribers. It's the backbone of the architecture. Technologies like **RabbitMQ**, **Apache Kafka**, **Redis Pub/Sub**, or **Google Cloud Pub/Sub** are implementations of this [[broker]].
3.  **Subscriber:** The entity that subscribes to one or more topics to receive the messages. The same message can be consumed by multiple subscribers (one-to-many communication).

**Typical Data Flow:**
* A publisher sends a message to a specific topic on the [[broker|message broker]].
* The [[broker]] receives and stores the message.
* It either notifies (pushes) or allows subscribers to retrieve it (pulls).
* Each interested subscriber receives a copy of the message and can process it independently.

---

## **Advantages and Technical Challenges**

* **Advantages (Benefits):**
    * **Low [[cohesion-coupling|Coupling]]:** Services are independent and can be developed, deployed, and scaled autonomously.
    * **Resilience:** If a subscriber is offline, the [[broker]] can queue messages until it becomes available again. This prevents data loss and makes the system more robust.
    * **Scalability:** It's simple to add new functionalities by creating new subscribers that listen to existing topics. You can also add multiple instances of the same subscriber to handle a higher message volume.
    * **Flexibility:** Enables **one-to-many** communication without additional complexity on the publisher side.

* **Challenges:**
    * **Latency:** The Pub/Sub model is asynchronous by nature, which introduces a delay between a message being sent and its reception. This is not a flaw, but an **architectural trade-off** that must be considered for applications requiring very low latency (e.g., high-frequency trading).
    * **Operational Complexity:** The [[broker]] introduces a new component to manage, monitor, and secure.
    * **[[broker|Broker]] Dependency:** If the [[broker]] fails (SPOF), communication between all services is interrupted. High availability and clustering solutions are therefore essential.
    * **Message Durability:** Persisting messages in case of a [[broker]] crash needs to be carefully managed, which can impact performance.
    * **Message Ordering:** Maintaining message order in a distributed system can be complex, especially if messaging is partitioned.

---

## **Comparison with [[message-queue|Message Queues and Streams]]**

| Characteristic | [[message-queue|Message Queues]] (e.g., RabbitMQ) | [[message-queue|Message Streams]] (e.g., Apache Kafka) | Publish/Subscribe (e.g., GCP Pub/Sub) |
| :--- | :--- | :--- | :--- |
| **Communication Model** | Point-to-point (one-to-one) | Broadcast (one-to-many) | Broadcast (one-to-many) |
| **Consumption Logic** | A single consumer retrieves and **deletes** the message from the queue. The message is not reusable. | Multiple consumers can read the **same** message in parallel, and keep track of their own **offset**. The message remains available for other readers. | The [[broker]] ensures that each subscribed consumer receives a delivery of the message. |
| **Message Persistence** | Messages are generally deleted after consumption (limited lifespan). | Messages are stored **persistently** in an ordered log. Messages can be **replayed** at any time. | Persistence can vary. It is often less durable than in streams, focusing on real-time delivery rather than historical storage. |
| **Use Cases** | Asynchronous tasks (e.g., sending emails, image processing), worker pools, load distribution. | Data stream processing (IoT), event logging, **Event Sourcing**, data pipelines. | Notifications, events, real-time data broadcasting (e.g., stock market updates, chat messages). |
| **Historical Relationship** | Not designed for history. | It is an event log by nature. The history of messages is a key feature. | History is not its primary function. It focuses on immediate dissemination. |

---

## **Variations and Derived Architectures**

* **Point-to-Point Messaging ([[message-queue|Message Queues]]):** Often contrasted with Pub/Sub. A message is consumed by only one consumer. Queues are useful for distributing tasks to a pool of workers. The **[[client-server|Client-Server]]** pattern can rely on a [[message-queue|message queue]] for its communication.
* **[[event-driven|Event-Driven Architecture (EDA)]]:** Pub/Sub is the basic pattern of EDA. Systems are designed around events, where each business action is an event (e.g., `OrderCreated`). Services react to these events to execute their own logic.
* **CQRS (Command and Query Responsibility Segregation):** Can use Pub/Sub to broadcast "events" which are the results of "commands". The "Query" service can then listen to these events to update its own data model optimized for reading.

This pattern, with its variations, is at the heart of **[[microservices]]** and real-time data processing systems. Its ability to decouple services makes it indispensable for designing modern and resilient systems.

---

## **Resources & links**

### **Articles**

1.  **[What is Pub/Sub?](https://www.geeksforgeeks.org/system-design/what-is-pub-sub/)**

    This **GeeksforGeeks** article provides an introduction to the **Publish-Subscribe (Pub/Sub)** messaging pattern. It explains how this asynchronous messaging pattern works by decoupling senders (publishers) from receivers (subscribers) through a [[broker|message broker]], enhancing scalability and reliability in system design.

2.  **[What is the Publish Subscribe Pattern?](https://www.contentful.com/blog/publish-subscribe-pattern/)**

    This **Contentful** article explains the **Publish-Subscribe pattern**, a common asynchronous messaging paradigm. It defines publishers, subscribers, and [[broker|message brokers]], detailing how this pattern facilitates loose coupling and enables efficient communication between different parts of a system, particularly in event-driven architectures.

---

### **Videos**

1.  **[What is the Publish/Subscribe Pattern? (Pub/Sub)](https://www.youtube.com/watch?v=algmP8MGeL4)**

    This video from **Coder Grrrl** explains the **Publish/Subscribe (Pub/Sub)** pattern, focusing on its core components: publishers, subscribers, and the [[broker|message broker]]. It illustrates how Pub/Sub enables decoupled communication between services, making systems more scalable and resilient by removing direct dependencies.

2.  **[Publish Subscribe Pattern - Software Design Pattern](https://www.youtube.com/watch?v=O1PgqUqZKTA)**

    In this video, **ByteByteGo** introduces the **Publish-Subscribe (Pub/Sub)** software design pattern. It delves into the working mechanism of Pub/Sub, highlighting how it facilitates asynchronous communication and decoupling between different system components, which is crucial for building scalable and robust distributed systems.
