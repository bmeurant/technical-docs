---
title: API Paradigms
tags:
  - api-paradigm
  - index
date: 2025-10-21
---

This section covers different paradigms for designing and building Application Programming Interfaces (APIs). These paradigms define the rules, styles, and patterns for how clients and servers communicate.

Choosing the right API paradigm is a critical architectural decision that impacts performance, scalability, and developer experience.

## Paradigms

- [[rpc|Remote Procedure Call (RPC)]]
- [[rest|REST (Representational State Transfer)]]
- [[graphql|GraphQL]]

## Paradigm Comparison

Choosing the right API paradigm is a critical architectural decision. Here is a high-level comparison between the main paradigms discussed in this section.

| Feature         | REST                                       | RPC                                           | GraphQL                                   |
|-----------------|--------------------------------------------|-----------------------------------------------|-------------------------------------------|
| **Style**       | Resource-oriented (nouns)                  | Action-oriented (verbs)                       | Query-based (declarative data fetching)   |
| **Interface**   | Uniform (GET, POST, PUT on URIs)           | Specific (e.g., `getUser()`, `createOrder()`) | Single endpoint with a flexible query language |
| **Data Fetching**| Fixed data structure per resource         | Defined by the procedure's return value       | Client specifies the exact data it needs  |
| **Coupling**    | Loosely coupled                            | Tightly coupled                               | Loosely coupled (schema-based)            |
| **Discovery**   | HATEOAS                                    | Requires documentation or IDL                 | Introspective schema                      |
| **Use Case**    | Public APIs, CRUD operations               | Internal microservices, command-based actions | Mobile apps, complex frontends, aggregating services |
