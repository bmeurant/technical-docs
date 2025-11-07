---
title: JWT (JSON Web Token)
tags:
  - security
  - authentication
  - jwt
  - token
date: 2025-11-02
---
# JSON Web Token (JWT)

A JSON Web Token (JWT) is a compact, URL-safe standard (RFC 7519) for creating self-contained access tokens that assert a number of claims. Because all necessary information is in the token itself, JWTs allow for stateless, scalable authentication systems, making them a cornerstone of modern [[authentication#Token-Based Authentication|token-based authentication]].

## Structure of a JWT

A JWT consists of three parts separated by dots (`.`): a Header, a Payload, and a Signature.

`xxxxx.yyyyy.zzzzz`

```
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c
```

### 1. Header
The header is a JSON object that declares the token type (`typ`) and the signing algorithm (`alg`) used, such as `HS256` (HMAC using SHA-256) or `RS256` (RSA). This JSON is then **Base64Url encoded** to form the first part of the JWT.

**Example Header (JSON):**
```json
{
  "alg": "HS256",
  "typ": "JWT"
}
```

### 2. Payload
The payload is a JSON object containing the **claims**, which are statements about an entity (typically the user) and additional metadata. Like the header, the payload is **Base64Url encoded** to form the second part of the JWT.

**Types of Claims:**
*   **Registered Claims**: A standard set of predefined claims, such as `iss` (issuer), `exp` (expiration time), `sub` (subject/user ID), and `iat` (issued at). These are recommended but not mandatory.
*   **Public Claims**: Custom claims defined by the application. To avoid collisions, they should be defined in the IANA JSON Web Token Registry or as a URI.
*   **Private Claims**: Custom claims created to share information between parties that agree on their meaning (e.g., an internal user ID).

**Example Payload (JSON):**
```json
{
  "sub": "1234567890",
  "name": "John Doe",
  "admin": true,
  "exp": 1516239022
}
```

### 3. Signature
The signature is used to verify the token's authenticity. It is created by taking the encoded header, the encoded payload, a secret key (if using HMAC) or a private key (if using RSA/ECDSA), and signing them with the algorithm specified in the header.

```
HMACSHA256(
  base64UrlEncode(header) + "." +
  base64UrlEncode(payload),
  secret
)
```
This signature ensures that the token has not been tampered with. The server verifies it using the same secret or the corresponding public key.

## How JWT Authentication Works

```mermaid
sequenceDiagram
    participant User
    participant ClientApp as Client Application
    participant Server

    User->>ClientApp: 1. Enters credentials (e.g., username/password)
    ClientApp->>Server: 2. Sends credentials for authentication
    Server->>Server: 3. Verifies credentials
    alt Credentials are valid
        Server->>Server: 4. Creates and signs a JWT
        Server-->>ClientApp: 5. Returns the JWT
        ClientApp->>ClientApp: 6. Stores the JWT securely (e.g., in memory)
    end

    Note over ClientApp, Server: For subsequent requests:

    ClientApp->>Server: 7. Sends JWT in Authorization header<br>(Authorization: Bearer __token__)
    Server->>Server: 8. Verifies JWT signature and claims (e.g., expiration)
    alt Token is valid
        Server-->>ClientApp: 9. Grants access to the requested resource
    else Token is invalid
        Server-->>ClientApp: 10. Rejects request (e.g., 401 Unauthorized)
    end
```

## Security Considerations

-   **Stateless but Irrevocable**: Once a JWT is issued, it is valid until it expires. It cannot be easily revoked without implementing a more complex, stateful blocklist on the server. This is a critical trade-off for the scalability of statelessness.
-   **Payload is Visible**: The payload is only encoded, **not encrypted**. Anyone can decode it and see its contents. Never store sensitive information (like passwords or personal secrets) in the payload.
-   **Signature is Critical**: The security of a JWT relies on its signature. The server **must** always verify the signature. A past vulnerability involved tokens specifying `"alg": "none"`, which could trick libraries into skipping verification. Always use up-to-date, well-maintained libraries.

## Related Concepts

-   [[authentication|Authentication]]: JWT is a common technology for implementing token-based authentication.
-   [[oauth|OAuth 2.0]]: JWTs are often used as bearer tokens in OAuth 2.0 flows.
-   [[hashing-algorithms|Hashing Algorithms]]: Used to create the JWT signature.

---

## Resources & Links

### Articles

1.  **[JWT Authentication Guide - roadmap.sh](https://roadmap.sh/guides/jwt-authentication)**
    A visual and straightforward guide that explains the purpose of JWTs, their structure, and how they fit into an authentication workflow.

2.  **[Introduction to JSON Web Tokens - JWT.io](https://jwt.io/introduction)**
    The official introduction from JWT.io. It explains the fundamental workflow of JWTs, from user login to the server creating a signed token, and how the client sends it back for subsequent requests. It's the canonical source for understanding the basic mechanism.

3.  **[What Is a JWT? - Postman Blog](https://blog.postman.com/what-is-jwt/)**
    A practical guide from the Postman blog that breaks down what a JWT is and why it's used. It covers the three parts of a JWT (Header, Payload, Signature) and provides a clear explanation of how stateless authentication with tokens works in modern APIs.

### Videos

1.  **[JWT Explained - YouTube](https://www.youtube.com/watch?v=7Q17ubqLfaM)**
    A clear and concise video that visually explains the structure of a JSON Web Token and its role in authentication. It walks through the process of signing and verifying a token, making the abstract concepts easy to grasp.

2.  **[What is JWT and How does it work? - YouTube](https://www.youtube.com/watch?v=P2CPd9ynFLg)**
    This video provides a detailed walkthrough of JWTs, covering the token structure, the difference between encoding and signing, and the complete authentication flow. It's a great resource for understanding both the "what" and the "how" of using JWTs.
