---
title: API Security Best Practices
tags:
  - api
  - security
  - design-principles
  - best-practices
date: 2025-11-03
---
# API Security Best Practices

> This checklist is inspired by the [API Security Best Practices](https://roadmap.sh/best-practices/api-security) from roadmap.sh.

This document provides a comprehensive checklist of security best practices to follow when designing, developing, and deploying APIs. It serves as a practical guide to mitigate common vulnerabilities and build a robust security posture. The principles are grouped by functional area, from authentication to data output.

---

## Authentication

Verifying the identity of clients is the first line of defense.

-   **Use Standard, Strong Authentication**: Avoid "Basic Authentication". Prefer token-based authentication using standards like [[jwt|JWT]] or session-based mechanisms. For third-party access, use a standard protocol like OAuth 2.0.
-   **Protect Against Brute-Force Attacks**: Implement rate limiting on login endpoints. After a certain number of failed attempts from a single IP or for a single user, block requests for a period (e.g., "Max Retry" and jail features).
-   **Don't Reinvent the Wheel**: Use well-vetted, standard libraries and frameworks for handling authentication logic.

> For a detailed overview of different strategies, see [[authentication]].

---

## Access Control

Once authenticated, enforce what users are allowed to do.

-   **Implement Rate Limiting**: Protect your API from Denial-of-Service (DoS) attacks and brute-force attempts by limiting the number of requests a client can make in a given time window.
    > See [[rate-limiting]] for detailed patterns.
-   **Enforce HTTPS and Modern TLS**: All API traffic must be encrypted in transit. Use [[ssl-tls|TLS 1.2 or 1.3]] with strong cipher suites.
-   **Use HSTS Header**: Implement the `Strict-Transport-Security` (HSTS) header to force browsers to communicate with your server only over HTTPS, preventing SSL stripping attacks.
-   **Use IP Whitelisting for Private APIs**: If an API is not meant for public consumption, restrict access at the network level to a list of trusted IP addresses using a [[firewalls|firewall]] or security group.
-   **Turn Off Directory Listings**: Configure your web server to prevent it from listing the contents of directories that don't have an index file.

---

## Input Validation

Treat all client input as untrusted.

-   **Validate `Content-Type`**: Ensure that the request's `Content-Type` header matches what your API expects (e.g., `application/json`). Reject requests with unexpected or missing headers.
-   **Validate All User Input**: Rigorously validate all incoming data (URL parameters, request body, headers) for format, type, length, and range to prevent common vulnerabilities like Injection attacks.
    > See the [[owasp|OWASP Top 10]] for common attack vectors.
-   **Use the `Authorization` Header**: Sensitive credentials like API keys or JWTs should be sent in the `Authorization` HTTP header, not in the URL or request body.
-   **Disable XML Entity Parsing**: If you must process XML, disable the parsing of external entities (XXE) and entity expansion to prevent related attacks.

---

## Processing & Business Logic

Secure the core logic of your application.

-   **Use Non-Sequential IDs in Public URLs**: Avoid using auto-incrementing integers as resource identifiers in public-facing URLs (e.g., `/users/123`). They are guessable and can lead to data leakage through enumeration attacks. Prefer UUIDs instead.
-   **Protect All Endpoints**: Ensure that no public endpoint is left unprotected by mistake. All endpoints should require authentication unless explicitly designed to be public. This relates to the "Broken Access Control" risk in [[owasp|OWASP]].
-   **Turn Off Debug Mode in Production**: Debug modes often provide verbose error messages that can leak sensitive information about your application's internal workings.
-   **Use a CDN for File Uploads**: Offload file uploads to a Content Delivery Network (CDN) to isolate potentially malicious files from your core application servers.

---

## Output Security

Control the data that your API sends back to the client.

-   **Use Security Headers**:
    -   `X-Content-Type-Options: nosniff`: Prevents the browser from MIME-sniffing a response away from the declared content-type.
    -   `X-Frame-Options: deny`: Protects against clickjacking attacks by preventing your pages from being rendered in an `<iframe>`.
    -   `Content-Security-Policy`: Define a strict policy to mitigate Cross-Site Scripting (XSS) and other injection attacks. See [[csp|Content Security Policy]].
-   **Remove Fingerprinting Headers**: Do not reveal specific technology details in headers like `X-Powered-By`, `Server`, or `X-AspNet-Version`.
-   **Never Return Raw Sensitive Data**: Avoid returning sensitive data like credentials, session tokens, or personally identifiable information (PII) in API responses.
-   **Return Proper Response Codes**: Use standard HTTP status codes to indicate the outcome of the request, but ensure error messages do not leak internal system details.

---

## Monitoring & Logging

You can't protect against what you can't see.

-   **Use Centralized Logging**: Aggregate logs from all services and components to get a unified view of activity.
-   **Do Not Log Sensitive Data**: Ensure that passwords, API keys, tokens, and other sensitive information are never written to logs, not even temporarily.
-   **Monitor for Suspicious Activity**: Use agents and alerts to monitor for high rates of errors, failed logins, or unusual request patterns that could indicate an attack.
    > See [[observability/monitoring]] for more on monitoring strategies.
-   **Use an Intrusion Detection/Prevention System (IDS/IPS)**: For high-security environments, an IDS/IPS can monitor network traffic for known attack signatures.
