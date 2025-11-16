---
title: ACID Transactions
tags:
  - databases
  - transactions
  - reliability
  - consistency
date: 2025-11-01
---

# ACID Transactions

**ACID** is a set of properties of database transactions intended to guarantee data validity despite errors, power failures, and other mishaps. In the context of databases, a single logical operation on the data is called a transaction. For example, a transfer of funds from one bank account to another, even though it involves multiple changes (debiting one account and crediting another), is a single transaction.

A classic example is a bank transfer.

```sql
BEGIN TRANSACTION;

-- Debit from Account A
UPDATE Accounts SET balance = balance - 100 WHERE account_id = 'A';

-- Credit to Account B
UPDATE Accounts SET balance = balance + 100 WHERE account_id = 'B';

COMMIT;
```

If the system crashes after the debit but before the credit, **Atomicity** ensures that the `BEGIN TRANSACTION` block is rolled back, and no money is lost.

The ACID model provides a powerful guarantee of reliability and consistency.

-   **Atomicity:** Ensures that all operations within a transaction are completed successfully as a single, indivisible unit. If any part of the transaction fails, the entire transaction is rolled back. It's all or nothing.
-   **Consistency:** Guarantees that a transaction brings the database from one valid state to another. All [[data-integrity|data integrity]] rules (like constraints and foreign keys) must be satisfied.
-   **Isolation:** Ensures that concurrent transactions do not interfere with each other. The result of concurrent transactions is the same as if they were executed sequentially. This prevents issues like dirty reads, non-repeatable reads, and phantom reads.
-   **Durability:** Guarantees that once a transaction has been committed, it will remain committed, even in the event of a system failure like a power outage or crash.

---

## Isolation Levels

Isolation is not an absolute property; it is implemented in different levels, which provide varying degrees of protection against concurrency issues. Lower levels offer better performance at the cost of weaker consistency guarantees.

1.  **Read Uncommitted**: The lowest level. One transaction can read uncommitted changes made by another. This can lead to **dirty reads**.
2.  **Read Committed**: The default for many databases (e.g., PostgreSQL, SQL Server). It prevents dirty reads by ensuring a transaction can only read data that has been committed. However, it can still lead to **non-repeatable reads** (the same row is read twice with different values).
3.  **Repeatable Read**: A higher level that prevents both dirty reads and non-repeatable reads. It ensures that if a row is read multiple times in a transaction, the result is always the same. However, it can still suffer from **phantom reads** (new rows are inserted by another transaction and become visible).
4.  **Serializable**: The highest level of isolation. It ensures that concurrent transactions have the same effect as if they were run one after another. It prevents all concurrency issues, including phantom reads, but it comes with the highest performance overhead.

---

## Trade-offs and the BASE Alternative

The strong guarantees of ACID compliance come at a cost, primarily in terms of performance and [[software-architecture/system-design-fundamentals/index#Scalability|scalability]]. Enforcing these properties, especially across a distributed system, can create significant overhead and latency.

This led to the development of an alternative model, particularly popular in the [[nosql|NoSQL]] world: **BASE**.

-   **B**asically **A**vailable: The system guarantees availability.
-   **S**oft state: The state of the system may change over time, even without input.
-   **E**ventually consistent: The system will become consistent over time, given that the system doesn't receive further input.

The BASE model prioritizes availability and [[software-architecture/system-design-fundamentals/index#Scalability|scalability]] over the strict consistency of ACID, making it a better fit for use cases like social media feeds or analytics, where some temporary inconsistency is acceptable.

---

## Resources & links

### Articles

1.  **[What Are ACID Transactions? A Complete Guide for Beginners](https://www.datacamp.com/blog/acid-transactions)**
    This article from DataCamp provides a comprehensive introduction to ACID properties, explaining each of the four principles (Atomicity, Consistency, Isolation, Durability) with clear examples. It also discusses the trade-offs of ACID, particularly in distributed systems, and contrasts it with the BASE model used in many NoSQL databases.

2.  **[ACID Properties Explained: Building Reliable Database Transactions](https://medium.com/@artemkhrenov/acid-properties-explained-building-reliable-database-transactions-08fdeb9d3153)**
    This Medium article offers a detailed explanation of the ACID properties, emphasizing their role in ensuring data integrity and reliability in database transactions. It breaks down each property with practical examples and discusses how they prevent common data anomalies in concurrent systems.

### Videos

1.  **[ACID Properties in Databases With Examples](https://www.youtube.com/watch?v=GAe5oB742dw)**
    This video provides a clear and concise explanation of the four ACID properties. It uses simple analogies and examples to illustrate how Atomicity, Consistency, Isolation, and Durability work together to guarantee reliable database transactions.

2.  **[Intro to ACID Database Transactions | Systems Design Interview](https://www.youtube.com/watch?v=oGmxzUBCYtY)**
    This video, framed as a system design interview topic, breaks down the ACID properties in a practical and easy-to-understand manner. It explains the importance of each property in maintaining data consistency and reliability, making it a great resource for both beginners and those preparing for technical interviews.
