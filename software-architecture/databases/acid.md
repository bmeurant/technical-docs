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

The ACID model provides a powerful guarantee of reliability and consistency.

-   **Atomicity:** Ensures that all operations within a transaction are completed successfully as a single, indivisible unit. If any part of the transaction fails, the entire transaction is rolled back. It's all or nothing.
-   **Consistency:** Guarantees that a transaction brings the database from one valid state to another. All data integrity rules (like constraints and foreign keys) must be satisfied.
-   **Isolation:** Ensures that concurrent transactions do not interfere with each other. The result of concurrent transactions is the same as if they were executed sequentially. This prevents issues like dirty reads, non-repeatable reads, and phantom reads.
-   **Durability:** Guarantees that once a transaction has been committed, it will remain committed, even in the event of a system failure like a power outage or crash.
