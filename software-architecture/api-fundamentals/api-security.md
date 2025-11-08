---
title: API Security
tags:
  - api
  - security
  - best-practices
  - vulnerabilities
  - owasp
date: 2025-11-07
---
# API Security

Securing APIs is a critical aspect of modern application development. This document provides a comprehensive overview of API security, covering both common vulnerabilities that expose systems to risk and the essential best practices required to mitigate them. The content is heavily inspired by the **[[owasp|OWASP API Security Top 10]]**, which specializes in the unique vulnerabilities of APIs.

---

## Common API Vulnerabilities

This section describes some of the most critical security risks in APIs. Understanding these vulnerabilities is the first step toward building a robust defense.

### 1. Broken Object Level Authorization (BOLA)

This is the most common and severe API vulnerability. It occurs when an API endpoint allows an authenticated user to access resources (objects) they are not authorized to see or modify, simply by changing the ID of the resource in the request.

*   **Example**: An attacker authenticated as `user_123` makes a request to `/api/orders/xyz`. They then change the request to `/api/orders/abc`, which belongs to another user, and the server returns the data without checking if `user_123` is the owner of order `abc`.
*   **Mitigation**: Implement strict and explicit [[authorization]] checks for every request that accesses a resource. Never rely on IDs sent by the client without verifying ownership.

### 2. Broken Authentication

This category covers weaknesses in how the client's identity is authenticated. This can include weak password policies, insecure session management, or flawed [[jwt|JWT]] validation.

*   **Example**: An API does not implement [[rate-limiting]] on its login endpoint, allowing an attacker to perform a brute-force attack by trying thousands of passwords per minute.
*   **Mitigation**: Use standard, strong authentication protocols like [[oauth|OAuth 2.0]]. Enforce multi-factor [[authentication]] (MFA), implement password complexity rules, and use secure token-based authentication.

### 3. Broken Property Level Authorization & Excessive Data Exposure

This vulnerability has two sides:
*   **Excessive Data Exposure**: The API returns more data in a response than is necessary for the client application's function. Even if the extra data is not displayed in the UI, it can be intercepted and analyzed by an attacker.
*   **Broken Property Level Authorization (formerly Mass Assignment)**: The API allows a client to set or update properties of an object that they should not have access to (e.g., changing a user's `isAdmin` flag from `false` to `true` by adding it to a JSON request body).

*   **Mitigation**:
    *   For exposure, design API responses to return only the minimal data required. Never rely on the client to filter sensitive data.
    *   For property-level authorization, use a "deny by default" approach. Explicitly define which fields are client-settable. Avoid automatically binding incoming data to internal objects.

### 4. Lack of Resources & Rate Limiting

If an API does not impose limits on the number or frequency of requests, it can be overwhelmed by Denial-of-Service (DoS) attacks or simple programming errors, leading to performance degradation or a complete outage.

*   **Example**: A malicious script calls a computationally expensive API endpoint in an infinite loop, consuming all available server CPU and memory.
*   **Mitigation**: Implement robust [[rate-limiting]] and throttling on all API endpoints based on client IP, user ID, or API key.

### 5. Broken Function Level Authorization

This vulnerability is related to BOLA but occurs at a higher level. It happens when an API fails to properly restrict access to different functions or operations based on the user's role or permissions.

*   **Example**: A regular user discovers an administrative endpoint like `/api/admin/deleteUser` and is able to successfully call it because the endpoint only checked for authentication, not for administrative privileges.
*   **Mitigation**: Your [[authorization]] logic must be granular. Define clear roles and permissions and verify them on the server-side for every sensitive operation. Deny all access by default.

---

## API Security Best Practices

This section provides a comprehensive checklist of security best practices to follow when designing, developing, and deploying APIs.

### Authentication

Verifying the identity of clients is the first line of defense.

-   **Use Standard, Strong Authentication**: Avoid "Basic Authentication". Prefer token-based authentication using standards like [[jwt|JWT]] or session-based mechanisms. For third-party access, use a standard protocol like [[oauth|OAuth 2.0]].
-   **Protect Against Brute-Force Attacks**: Implement rate limiting on login endpoints. After a certain number of failed attempts from a single IP or for a single user, block requests for a period.
-   **Don't Reinvent the Wheel**: Use well-vetted, standard libraries and frameworks for handling authentication logic.
    > For a detailed overview of different strategies, see [[authentication]].

### Access Control

Once authenticated, enforce what users are allowed to do.

-   **Implement Rate Limiting**: Protect your API from DoS attacks and brute-force attempts by limiting requests.
    > See [[rate-limiting]] for detailed patterns.
-   **Enforce HTTPS and Modern TLS**: All API traffic must be encrypted in transit. Use [[ssl-tls|TLS 1.2 or 1.3]] with strong cipher suites.
-   **Use HSTS Header**: Implement the `Strict-Transport-Security` header to force browsers to communicate with your server only over HTTPS.
-   **Use IP Whitelisting for Private APIs**: If an API is not public, restrict access at the network level using a [[firewalls|firewall]].

### Input Validation

Treat all client input as untrusted.

-   **Validate `Content-Type`**: Ensure the request's `Content-Type` header matches what your API expects (e.g., `application/json`). Reject unexpected types.
-   **Validate All User Input**: Rigorously validate all incoming data (URL parameters, request body, headers) for format, type, length, and range to prevent Injection attacks.
-   **Use the `Authorization` Header**: Sensitive credentials like API keys or JWTs should be sent in the `Authorization` HTTP header.
-   **Disable XML Entity Parsing**: If you must process XML, disable external entity parsing (XXE) to prevent related attacks.

### Processing & Business Logic

Secure the core logic of your application.

-   **Use Non-Sequential IDs in Public URLs**: Avoid auto-incrementing integers as resource identifiers (e.g., `/users/123`). They are guessable and can lead to enumeration attacks (BOLA). Prefer UUIDs instead.
-   **Protect All Endpoints**: Ensure that all endpoints require authentication unless explicitly public. This relates to the "Broken Access Control" risk.
-   **Turn Off Debug Mode in Production**: Verbose error messages can leak sensitive information about your application's internals.

### Output Security

Control the data that your API sends back to the client.

-   **Use Security Headers**: Implement headers like `X-Content-Type-Options: nosniff`, `X-Frame-Options: deny`, and a strict [[csp|Content Security Policy]].
-   **Remove Fingerprinting Headers**: Do not reveal technology details in headers like `X-Powered-By` or `Server`.
-   **Never Return Raw Sensitive Data**: Avoid returning credentials, session tokens, or PII in API responses.
-   **Return Proper Response Codes**: Use standard HTTP status codes, but ensure error messages do not leak internal system details.

### Monitoring & Logging

You can't protect against what you can't see.

-   **Use Centralized Logging**: Aggregate logs from all services to get a unified view of activity.
-   **Do Not Log Sensitive Data**: Ensure that passwords, API keys, tokens, and other sensitive information are never written to logs.
-   **Monitor for Suspicious Activity**: Use alerts to monitor for high error rates, failed logins, or unusual request patterns.
    > See [[observability/monitoring]] for more on monitoring strategies.
