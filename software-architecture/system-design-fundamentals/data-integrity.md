---
title: Data Integrity
tags:
  - databases
  - system-design
  - reliability
date: 2025-11-11
---

# Data Integrity

**Data Integrity** is the maintenance of, and the assurance of the accuracy and consistency of, data over its entire life-cycle. It is a critical aspect of the design, implementation, and usage of any system that stores, processes, or retrieves data. In short, data integrity is about ensuring data is correct, consistent, and reliable.

When data integrity is high, the data in the database is the same as what is in the real world, and it remains so over time. This is a core goal of database systems, particularly [[rdbms|Relational Databases (RDBMS)]].

---

## Types of Data Integrity

Data integrity is typically enforced by a series of integrity constraints or rules. These can be categorized into the following types.

### 1. Entity Integrity

**Entity Integrity** ensures that every table has a primary key and that the key is unique and not null. This guarantees that every row in a table can be uniquely identified, preventing duplicate or unidentified records.

-   **Constraint:** `PRIMARY KEY`
-   **Purpose:** To ensure each entity (row) is a unique, identifiable object.

**Example:**

```sql
CREATE TABLE Users (
    UserID INT PRIMARY KEY, -- The UserID must be unique and not NULL
    Name VARCHAR(100)
);
```

### 2. Referential Integrity

**Referential Integrity** is a rule that preserves the defined relationships between tables when records are entered or deleted. It ensures that a foreign key value in one table has a matching primary key value in the table it refers to. This prevents "orphan" records—records that reference a non-existent entity.

-   **Constraint:** `FOREIGN KEY`
-   **Purpose:** To maintain consistency between related tables.

**Example:**

```sql
CREATE TABLE Orders (
    OrderID INT PRIMARY KEY,
    UserID INT,
    Product VARCHAR(100),
    FOREIGN KEY (UserID) REFERENCES Users(UserID) -- Ensures every UserID in Orders exists in the Users table
);
```
If you try to insert an order with a `UserID` that does not exist in the `Users` table, the database will reject the operation.

### 3. Domain Integrity

**Domain Integrity** ensures that all values in a column adhere to a specific domain of valid values. This includes enforcing data types, formats, and ranges.

-   **Constraints:** `CHECK`, `NOT NULL`, `DEFAULT`, data types (e.g., `INT`, `VARCHAR`).
-   **Purpose:** To ensure that data values are of the correct type, format, and within a valid range.

**Example:**

```sql
CREATE TABLE Products (
    ProductID INT PRIMARY KEY,
    ProductName VARCHAR(100) NOT NULL,
    Price DECIMAL(10, 2) CHECK (Price > 0), -- Price must be a positive number
    Category VARCHAR(50) DEFAULT 'Uncategorized'
);
```

### 4. User-Defined Integrity

**User-Defined Integrity** allows for the enforcement of specific business rules that do not fall into the other categories. These are often implemented using triggers, stored procedures, or complex constraints.

-   **Constraints:** `TRIGGER`, complex `CHECK` clauses.
-   **Purpose:** To enforce business logic that is specific to the application.

**Example:**
A trigger could be used to prevent a customer from exceeding a certain credit limit by checking their total order value before allowing a new order to be inserted.

---

## Data Integrity vs. Data Security

While related, data integrity and data security are distinct concepts.
-   **Data Integrity** is about the *accuracy and consistency* of the data.
-   **Data Security** is about protecting data from *unauthorized access or corruption*.

For example, a security breach might compromise data integrity if an attacker maliciously alters data. However, data integrity can also be lost due to programming errors or hardware failures, with no security breach involved.
