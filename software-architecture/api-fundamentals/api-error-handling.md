---
title: API Error Handling
tags:
  - api
  - api-fundamentals
  - error-handling
  - rest
  - graphql
date: 2025-11-06
---
# API Error Handling

Consistent and informative error handling is a critical component of a well-designed API. A good strategy improves the developer experience, enabling consumers to understand and resolve issues efficiently. While the core principles are universal, the implementation details differ significantly between [[REST]], [[graphql|GraphQL]], and [[json-rpc|JSON-RPC]] APIs.

---

## Client-Side vs. Server-Side Errors

A fundamental concept in API error handling is the distinction between client-side and server-side errors. Understanding who is responsible for the error dictates how the server should respond.

### 1. Client-Side Errors

These errors occur when the client sends a request that is invalid, malformed, or for which it does not have permission. The problem lies with the request itself.

- **Cause:** Invalid input data, authentication failure, authorization failure, or requesting a non-existent resource.
- **Responsibility:** It is the **client's responsibility** to fix the request before retrying.
- **Server's Goal:** To provide a clear, actionable error message so the client developer can debug and correct their request.
- **Analogy:** In REST, this corresponds to the `4xx` family of HTTP status codes. In GraphQL, these are often errors with specific extension codes like `BAD_USER_INPUT` or `UNAUTHENTICATED`.

### 2. Server-Side Errors

These errors occur when the client's request was valid, but the server failed to process it due to an internal problem.

- **Cause:** A database is down, an uncaught exception occurred, a downstream service is unavailable, or any other internal bug.
- **Responsibility:** It is the **server's responsibility** to fix the issue. The client can do nothing but retry the request later.
- **Server's Goal:** To acknowledge the failure gracefully **without exposing any sensitive internal details**. The best practice is to log the internal error and return a generic message with a unique ID for tracking.
- **Analogy:** In REST, this corresponds to the `5xx` family of HTTP status codes. In GraphQL, this is typically an `INTERNAL_SERVER_ERROR` code in the error's `extensions` object.

---

## The Client's Role: Proactive Error Prevention

The best error is one that never occurs. A robust client application should practice [[defensive-programming|defensive programming]] and prevent easily avoidable errors by validating user input *before* sending it to the API.

### Client-Side Validation

- **What it is:** The practice of checking data on the client-side (e.g., in a web browser or mobile app) before making an API call. Examples include ensuring an email field has a valid format, a password meets complexity rules, or a required field is not empty.
- **Benefits:**
  - **Improved User Experience:** Provides immediate feedback to the user without a slow server round-trip.
  - **Reduced Server Load:** Prevents the server from wasting resources on obviously invalid requests.

### The Golden Rule: Never Trust the Client

Client-side validation is a **UX enhancement and an optimization, not a security measure.** The server **must always** re-validate all incoming data. A malicious actor can easily bypass any client-side checks and send raw, malicious data directly to the API endpoint. 

- **Client-Side Validation:** For user experience.
- **Server-Side Validation:** For security and data integrity.

---

## Core Principles of Server Responses

These fundamental practices apply to how a server should construct its error responses, regardless of the API paradigm.

### 1. Use a Structured, Predictable Format

Error responses should be machine-readable and follow a consistent structure across all endpoints and error types. This allows clients to parse errors programmatically. Avoid sending plain text or HTML error pages.

### 2. Don't Expose Sensitive Information

This is a critical security practice, especially for server-side errors. Error responses should **never** reveal internal implementation details that could be exploited by malicious actors.

- **What to Avoid:** Stack traces, database queries, internal variable names, or cryptic framework-specific error messages.
- **Why it Matters:** Exposing these details leads to information leakage vulnerabilities, giving attackers insights into your system's architecture and potential weaknesses.
- **Best Practice:** Log the detailed error on the server for debugging purposes, assign it a unique ID (a correlation ID), and return only that ID to the client. The client can then report this ID, allowing developers to find the corresponding detailed error in the logs.

### 3. Document Your Errors

Just as you document your API's features, you should document its possible errors. The documentation for each endpoint should list the potential error `types` it can return, what they mean, and how a client can resolve them. This empowers developers to write more resilient code that handles failure cases correctly.

### 4. Consider Localization

For public-facing APIs with a global audience, providing error messages in the client's native language can significantly improve the developer experience. This is an advanced practice, typically implemented by respecting the `Accept-Language` HTTP header and translating the `title` and `detail` fields of the error response.

---

## Error Handling in REST APIs

Error handling in REST is built on top of standard HTTP conventions, clearly mapping to the client-side vs. server-side model.

### 1. Use Standard HTTP Status Codes

HTTP status codes are the foundation of REST API error communication.

- **4xx (Client Errors):** The request cannot be fulfilled due to a client-side issue.
  - `400 Bad Request`: Generic error for malformed requests (e.g., invalid JSON).
  - `401 Unauthorized`: The client must authenticate.
  - `403 Forbidden`: The client is authenticated but does not have permission.
  - `404 Not Found`: The requested resource does not exist.
- **5xx (Server Errors):** The server failed to fulfill a valid request due to an internal issue.
  - `500 Internal Server Error`: A generic server error. Never expose the underlying cause.
  - `503 Service Unavailable`: The server is temporarily overloaded or down for maintenance.

### 2. The "Problem Details" Standard (RFC 7807)

To standardize structured error responses, **RFC 7807: Problem Details for HTTP APIs** defines a standard JSON object (`application/problem+json`).

- `type`: A URI identifying the specific error type (links to documentation).
- `title`: A short, human-readable summary.
- `status`: The HTTP status code.
- `detail`: A human-readable explanation specific to this occurrence.
- `instance`: A URI identifying this specific occurrence (e.g., a path to a log entry).

### Example REST Error Responses

**400 - Validation Error (Client-Side)**
```json
{
  "type": "https://api.example.com/errors/validation-error",
  "title": "Validation Failed",
  "status": 400,
  "detail": "The request body contains invalid parameters.",
  "instance": "/requests/abc-123",
  "invalid_params": [
    { "field": "email", "reason": "must be a valid email address" }
  ]
}
```

**500 - Internal Server Error (Server-Side)**
```json
{
  "type": "https://api.example.com/errors/internal-server-error",
  "title": "Internal Server Error",
  "status": 500,
  "detail": "An unexpected error occurred. Please report this error with ID: xyz-789.",
  "instance": "/errors/xyz-789"
}
```

---

## Error Handling in GraphQL

Error handling in [[graphql|GraphQL]] follows a different model. A GraphQL API will almost always return an `HTTP 200 OK` status code, even if errors occurred during the request processing.

### The GraphQL Error Model

The response from a GraphQL server is a JSON object that can contain both a `data` key and an `errors` key. This allows for partial success, where the server can return some of the requested data while also reporting errors for the fields it could not resolve.

### Structure of a GraphQL Error

The specification defines a standard structure for objects inside the `errors` array. The client vs. server distinction is made using the `extensions` object.

- `message`: A human-readable description of the error.
- `locations`: An array indicating the line and column in the original query where the error occurred.
- `path`: An array representing the path to the field in the query that produced the error.
- `extensions`: A custom object where you provide machine-readable data. This is where you should put a specific error code to distinguish between client and server errors.

### Example GraphQL Error Response

Imagine a query asks for a user's name and their latest post, but the post service is down (a server-side error).

```json
{
  "data": {
    "user": {
      "name": "Jane Doe",
      "latestPost": null
    }
  },
  "errors": [
    {
      "message": "The post service is currently unavailable.",
      "locations": [{ "line": 3, "column": 5 }],
      "path": ["user", "latestPost"],
      "extensions": {
        "code": "INTERNAL_SERVER_ERROR",
        "timestamp": "2025-11-06T10:30:00Z"
      }
    }
  ]
}
```
*Description: The API successfully returned the user's name but failed to retrieve the latest post. The `200 OK` response contains both the partial data and a detailed error object explaining the server-side failure.*

---

## Error Handling in JSON-RPC

Like GraphQL, the [[json-rpc|JSON-RPC]] paradigm also handles application-level errors in the response body while typically returning an `HTTP 200 OK` status. However, its approach is simpler and more rigid than GraphQL's.

### The JSON-RPC Error Object

If a request fails, the response object will contain an `error` member instead of a `result` member. The structure of this error object is strictly defined by the specification.

- `code`: An integer representing the error type (e.g., `-32602` for "Invalid Params").
- `message`: A string summary of the error.
- `data` (Optional): For application-specific error details.

This strict standardization ensures that clients can reliably parse errors. Unlike GraphQL, a JSON-RPC request typically fails as a whole; there is no concept of returning partial data.

**Example JSON-RPC Error Response:**
```json
{
  "jsonrpc": "2.0",
  "error": {
    "code": -32602,
    "message": "Invalid params",
    "data": "The 'age' parameter cannot be negative."
  },
  "id": 1
}
```
*Description: The server returns a `200 OK` status, but the response body clearly indicates a client-side error with the parameters, following the strict JSON-RPC error format. For more details, see the main [[json-rpc]] page.*

---

## Error Handling in Real-Time APIs

Error handling in [[real-time-communication|real-time communication patterns]] like WebSockets and SSE cannot rely on HTTP status codes for each message, as the connection is already established. Instead, errors must be communicated through the message protocol itself.

*   **Connection-Level Errors**: If an error occurs during the initial HTTP handshake (e.g., authentication failure), the server can and should respond with a standard `4xx` or `5xx` HTTP status code.
*   **Message-Level Errors**: Once the connection is established, errors must be sent as part of the data stream. A common pattern is to define a standard message format that includes an `error` field.

    **Example WebSocket Error Message:**
    ```json
    {
      "type": "error",
      "payload": {
        "code": 4001,
        "message": "Invalid message format"
      }
    }
    ```
*   **Closing the Connection**: For severe errors, the server can close the connection programmatically, sending a close code and reason. WebSocket close codes (e.g., `4000-4999` for application use) can be used to provide machine-readable information about why the connection was terminated.

---

## Resources & links

### Articles

1.  **[Best Practices for API Error Handling](https://blog.postman.com/best-practices-for-api-error-handling/)**
    This Postman article provides a comprehensive overview of error handling, contrasting the REST approach (using HTTP status codes) with the GraphQL model (using an `errors` array). It emphasizes the universal need for clear messages, robust validation, and good documentation for creating a positive developer experience.

2.  **[Error Handling in API Design](https://medium.com/@elijahechekwu/error-handling-in-api-design-c2d38cfc68d9)**
    This Medium article makes a strong case for consistency in API error design. It focuses on the core principles of using standard HTTP status codes correctly and returning a well-structured error body with machine-readable codes and human-readable messages, while strictly avoiding the exposure of internal server details.

3.  **[Best Practices for REST API Error Handling](https://www.baeldung.com/rest-api-error-handling-best-practices)**
    This detailed guide from Baeldung focuses on practical implementation for REST APIs. It champions the use of the RFC 7807 "Problem Details" standard and explains how to implement a custom exception handling mechanism on the server to map internal application exceptions to appropriate and informative HTTP error responses.
