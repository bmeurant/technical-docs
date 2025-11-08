--- 
title: Security
tags:
  - security
  - software-architecture
  - cybersecurity
  - information-security
date: 2025-11-07
---

# Security Concepts in Software Architecture

This section covers the key concepts, patterns, and technologies for building secure software systems. A strong understanding of security is a foundational requirement for any software architect or developer.

For a high-level overview of the core principles, start with the **[[security-fundamentals|Security Fundamentals]]** page.

---

## Topics

### Identity and Access Management (IAM)

This area focuses on verifying the identity of users and systems and controlling what they are allowed to do.

*   [[authentication|Authentication]]
*   [[authorization|Authorization]]
*   [[oauth|OAuth 2.0]]
*   [[openid-connect|OpenID Connect (OIDC)]]
*   [[jwt|JSON Web Token (JWT)]]

### Cryptography and Data Integrity

These topics cover the techniques used to protect data both in transit and at rest.

*   [[hashing-algorithms|Hashing Algorithms]]
*   [[pki|Public Key Infrastructure (PKI)]]
*   [[ssl-tls|SSL/TLS]]

### Application and Web Security

This section covers security from the perspective of a web application or service.

*   [[owasp|OWASP (Open Web Application Security Project)]]
*   [[cors|Cross-Origin Resource Sharing (CORS)]]
*   [[csp|Content Security Policy (CSP)]]

### Server and Network Security

This area covers practices to protect server infrastructure from threats.

*   [[firewalls|Firewalls]]

### Related Patterns

*   For specific, proven solutions to recurring security design problems, see the **[[software-architecture/security-patterns/|Security Patterns]]** section.
*   The **[[gatekeeper|Gatekeeper Pattern]]** is a relevant architectural pattern for centralizing security checks.
