---
title: Asynchronous Messaging Models
tags:
  - messaging
  - communication
date: 2025-09-20
---
# Asynchronous Messaging Models: Queues vs. Topics & Streams

Asynchronous messaging is the foundational communication method that enables modern, decoupled distributed systems. It relies on a central **[[broker]]** that acts as an intermediary between message **Producers** and **Consumers**.

This document compares the two fundamental low-level models of asynchronous communication. These models serve as the technical backbone for higher-level architectural styles like **[[message-driven|Message-Driven Architecture]]** and **[[event-driven|Event-Driven Architecture]]**.

---

## The Two Core Models

There are two primary models for asynchronous messaging, each serving a different purpose:

1.  **[[point-to-point-messaging|Point-to-Point (Queue)]]:** This model is used for **one-to-one** communication. A message is sent to a queue and delivered to a single consumer. It is ideal for distributing tasks and commands.

2.  **[[publish-subscribe|Publish-Subscribe (Topic or Stream)]]:** This model is used for **one-to-many** communication. A message is published to a central channel and broadcast to all interested subscribers. It is ideal for notifying different parts of a system about an event. The implementation of this channel can be a simple topic or a more robust **[[publish-subscribe#Topics vs. Event Streams|Event Stream]]**.

### High-Level Comparison

| Characteristic | **[[point-to-point-messaging|Point-to-Point (Queue)]]** | **[[publish-subscribe|Publish-Subscribe (Topic / Stream)]]** |
| :--- | :--- | :--- |
| **Communication Model** | **One-to-One**. A single message is processed by exactly one consumer. | **One-to-Many**. A single message is broadcast to all interested consumers. |
| **Consumption Logic** | **Destructive Read**. When a consumer processes a message, it is removed from the queue. | **Non-Destructive Read**. Consumers read from the channel without removing the message. |
| **Primary Use Case** | **Task/Command Distribution**. Ideal for distributing work among a pool of identical workers. | **Event Notification**. Ideal for broadcasting that something has happened, allowing different parts of a system to react independently. |

For a detailed explanation of each pattern, please refer to their dedicated documents:
*   **Deep Dive: [[point-to-point-messaging]]**
*   **Deep Dive: [[publish-subscribe]]**

---

## General Advantages of Asynchronous Messaging

* **[[cohesion-coupling|Decoupling]]**: Services have no direct dependency on one another, which simplifies system development, maintenance, and evolution.
* **Resilience**: If a consumer fails, messages are not lost; they remain in the **[[broker]]** until a service resumes processing.
* **Scalability**: The **[[broker]]** handles traffic spikes by acting as a buffer. It is easy to add more consumers to increase processing capacity without impacting the rest of the system.
* **Flexibility**: Different services can be developed using distinct technologies as long as they adhere to the common communication protocol with the **[[broker]]**.