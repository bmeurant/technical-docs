---
title: NoSQL Databases
tags:
  - system-design
  - databases
  - nosql
  - scalability
date: 2025-10-21
---

# NoSQL Databases

NoSQL, meaning "Not Only SQL," represents a diverse family of [[databases|database]] systems that break away from the rigid, tabular structure of traditional [[rdbms|relational databases]]. They emerged from the needs of web-scale applications at companies like Google, Amazon, and Facebook, which required massive [[system-design-fundamentals#Scalability|scalability]], high [[system-design-fundamentals#Performance-vs-Scalability|performance]], and flexibility that RDBMS struggled to provide.

This page covers the core concepts of NoSQL and details its most common data models.

---

## Key Characteristics of NoSQL Databases

NoSQL databases are defined by a different set of priorities compared to their relational counterparts, focusing on speed, scale, and flexibility.

### 1. Flexible Data Models

The most defining characteristic of NoSQL databases is their flexible data model. Unlike the strict, predefined schemas of SQL, NoSQL databases allow you to store data without a fixed structure. This is often called "schema-on-read"—the data has a structure, but the database doesn't enforce it. This allows you to store varied data types in the same collection and add new fields on the fly, providing immense flexibility for rapidly evolving applications.

### 2. Horizontal Scalability (Scaling Out)

NoSQL databases are designed from the ground up to [[system-design-fundamentals#Scalability|scale horizontally]]. This means they can handle increased load by adding more commodity servers to a cluster, a process known as sharding. This architecture is what allows NoSQL databases to handle massive volumes of data and high traffic loads, a feat that is often complex and expensive to achieve with traditional RDBMS.

### 3. High Performance and Availability

By simplifying their data models and often relaxing the strict consistency guarantees of ACID, NoSQL databases can achieve extremely high performance for specific workloads, especially large-volume reads and writes. They are typically optimized for availability, meaning the system remains operational for both reads and writes even if some nodes in the cluster fail. This aligns with the **BASE** model:

-   **B**asically **A**vailable: The system guarantees [[system-design-patterns/availability-patterns|availability]].
-   **S**oft State: The state of the system may change over time, even without input.
-   **E**ventually Consistent: The system will become [[system-design-patterns/consistency-patterns#Eventual-Consistency|eventually consistent]].

This approach is a direct trade-off, as explained in the [[cap|CAP Theorem]], prioritizing availability over strong consistency.

### 4. API-Driven and Developer-Friendly

Most NoSQL databases are designed to be used by developers via simple, API-driven calls. The data formats they use, such as JSON in document databases, map directly to the objects and data structures used in modern programming languages. This eliminates the "object-relational impedance mismatch"—the complex mapping layer (ORM) often required to translate between the object-oriented world of an application and the relational world of a SQL database.

---

## Common NoSQL Database Models

NoSQL is not a single type of database, but a category that includes several different models.

### 1. Key-Value Store

This is the simplest NoSQL model. Data is stored as a collection of key-value pairs, much like a dictionary or hash map. The value is treated as an opaque blob.

```mermaid
graph TD
    subgraph "Key-Value Store"
        K1("Key: 'user:123'") --> V1("Value: {name: 'Alice', ...}")
        K2("Key: 'session:abc'") --> V2("Value: {user_id: 123}")
    end
```
*Description: Each key maps to a single value. Retrieval is extremely fast when you know the key.*

-   **Operations:** Simple `get`, `set`, and `delete` operations.
-   **Use Cases:** Caching, session management, real-time leaderboards.
-   **Examples:** Redis, Amazon DynamoDB.

### 2. Document Store

A Document Store extends the key-value concept. The value is a structured "document," typically in a JSON or BSON format, which the database can understand and query.

```mermaid
graph TD
    subgraph "Document Store"
        K("Key: 'user:123'") --> D["Document<br>{
  _id: 123,<br>
  name: 'Alice',
  email: 'alice@example.com',
  orders: [ ... ]
}"]
    end
```
*Description: Data is stored in self-contained documents. The database can index and query fields within the document.*

-   **Operations:** Rich query language to filter documents based on their internal fields.
-   **Use Cases:** Content management systems, e-commerce product catalogs, user profiles.
-   **Examples:** MongoDB, Couchbase.

### 3. Wide-Column Store

Wide-Column stores organize data into tables, rows, and columns, but unlike an RDBMS, the names and format of the columns can vary from row to row in the same table. They are optimized for queries over large datasets.

```mermaid
graph TD
    subgraph "Wide-Column Store (Simplified)"
        R1["Row Key: user123<br>------------------<br>name: 'Alice'<br>email: 'a@b.com'<br>city: 'New York'"] 
        R2["Row Key: user456<br>------------------<br>name: 'Bob'<br>email: 'c@d.com'<br>last_login: '2025-10-20'"] 
    end
```
*Description: Rows are not required to have the same columns. This is ideal for storing data with varied attributes.*

-   **Operations:** Optimized for high-speed reads and writes on specific columns for a given row key.

-   **Use Cases:** Big Data analytics, IoT time-series data, logging systems.
-   **Examples:** Apache Cassandra, Google Bigtable, HBase.

### 4. Graph Database

Graph databases are purpose-built to store and navigate relationships. They use nodes (to store entities) and edges (to represent the relationships between entities).

```mermaid
graph TD
    subgraph "Graph Database"
        A("Node: Person<br>{name: 'Alice'}") -- "Edge: FRIENDS_WITH" --> B("Node: Person<br>{name: 'Bob'}")
        B -- "Edge: LIVES_IN" --> C("Node: City<br>{name: 'New York'}")
    end
```
*Description: The model focuses on the connections between data points, making relationship-based queries extremely efficient.*

-   **Operations:** Specialized query languages (e.g., Cypher for Neo4j) designed to traverse complex relationships efficiently.
-   **Use Cases:** Social networks, fraud detection, recommendation engines.
-   **Examples:** Neo4j, Amazon Neptune.

---

## Resources & links

### Articles

1.  **[What Is NoSQL? NoSQL Databases Explained - MongoDB](https://www.mongodb.com/resources/basics/databases/nosql-explained)**
    An article from MongoDB explaining the features, types, and benefits of NoSQL databases, contrasting them with relational models.

2.  **[What is a NoSQL Database? - Google Cloud](https://cloud.google.com/discover/what-is-nosql?hl=en)**
    A high-level overview of NoSQL databases and their use cases from Google Cloud.

### Videos

1.  **[How do NoSQL databases work? Simply Explained!](https://www.youtube.com/watch?v=0buKQHokLK8)**
    A short video explaining the core concepts of NoSQL, including horizontal scaling and the different data models.

2.  **[NoSQL Databases - Hussein Nasser](https://www.youtube.com/watch?v=qEhNHOEa5sE&list=PLsyeobzWxl7r0bn6dzVA8bQNxcx7DRl5F)**
    The first video of a detailed playlist from Hussein Nasser covering the fundamentals and various types of NoSQL databases.

