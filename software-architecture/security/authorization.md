---
title: Authorization
tags:
  - security
  - authorization
  - access-control
  - rbac
  - abac
  - dac
  - mac
  - rebac
date: 2025-11-07
---

# Authorization: Defining Access Rights

Authorization is the security process that determines an authenticated entity's level of access or the actions it is permitted to perform. While [[authentication]] is concerned with verifying identity ("Who are you?"), authorization is about granting permissions ("What are you allowed to do?"). It is the crucial step that follows a successful authentication.

In any secure system, from a simple blog to a complex enterprise application, authorization mechanisms are required to ensure that users and services can only access the specific data and functionality they are entitled to.

## Core Authorization Models

Several models exist for managing permissions. The choice of model depends on the system's complexity, [[software-architecture/system-design-fundamentals/index#Scalability|scalability]] requirements, and the granularity of control needed.

### Role-Based Access Control (RBAC)

RBAC is one of the most common authorization models. Access decisions are based on the roles assigned to a user.

*   **Mechanism**: Permissions are associated with roles (e.g., "create article," "delete user"). Users are then assigned one or more roles (e.g., "editor," "administrator"). A user can perform an action if one of their assigned roles contains the required permission.
*   **Example**: In a content management system:
    *   **Viewer Role**: Has `read_article` permission.
    *   **Editor Role**: Has `read_article`, `create_article`, and `edit_article` permissions.
    *   **Admin Role**: Has all editor permissions plus `delete_article` and `manage_users`.
*   **Pros**: Simple to understand and manage, especially in systems with a limited number of well-defined job functions.
*   **Cons**: Can become rigid. If many fine-grained permissions are needed, it can lead to "role explosion," where a vast number of roles must be created and managed.

### Policy-Based and Attribute-Based Access Control (PBAC & ABAC)

**Policy-Based Access Control (PBAC)** is a broad authorization paradigm where access decisions are made by evaluating a set of policies. Instead of assigning static permissions to roles, PBAC uses logical rules that can factor in a wide range of information to grant or deny access dynamically.

The most common and powerful implementation of PBAC is **Attribute-Based Access Control (ABAC)**.

*   **Mechanism**: ABAC implements a policy-based system where the rules are based on attributes. Policies evaluate attributes of the user, the resource being accessed, and the current environment to make a decision. These are often expressed as "IF-THEN" rules.
*   **Attributes can include**:
    *   **User Attributes**: Role, department, security clearance, age.
    *   **Resource Attributes**: Type (e.g., "financial report"), owner, sensitivity level, creation date.
    *   **Environmental Attributes**: Time of day, location of access (IP address), device security status.
*   **Example Policy**: "*Allow* users if their `department` is 'Finance' AND the `resource_type` is 'Financial Report' AND the `action` is 'view' AND the `time_of_day` is between 9 AM and 5 PM."
*   **Pros**: Extremely flexible, context-aware, and scalable for complex scenarios.
*   **Cons**: More complex to design, implement, and debug the policies.

### Discretionary Access Control (DAC)

In the DAC model, the owner of a resource is given the discretion to determine who is allowed to access it.

*   **Mechanism**: Every resource has an owner, and the owner can grant or revoke permissions for other users or groups. This is the model used by most consumer-facing systems and operating systems.
*   **Example**: In a file system like Linux or a service like Google Drive, the creator of a file (the owner) can set read/write/execute permissions for themselves, a group, and all other users.
*   **Pros**: Very flexible and intuitive for users.
*   **Cons**: Difficult to manage centrally. In large organizations, it can lead to inconsistent permissions and security vulnerabilities if users grant overly permissive access.

### Mandatory Access Control (MAC)

MAC is the most rigid model, where a central authority mandates access rights based on security labels.

*   **Mechanism**: Both subjects (users) and objects (resources) are assigned a security label (e.g., Unclassified, Confidential, Top Secret). Access is granted only if the subject's label is equivalent to or higher than the object's label, based on a system-wide policy.
*   **Example**: Used in military and high-security government systems. A user with "Secret" clearance can access "Secret" and "Unclassified" documents but is blocked from accessing a "Top Secret" document.
*   **Pros**: Provides the highest level of security and centralized control.
*   **Cons**: Inflexible, complex to manage, and generally unsuitable for commercial or collaborative environments.

### Relationship-Based Access Control (ReBAC)

ReBAC grants permissions based on the relationship between a user and a resource. It answers the question, "Can this user access this resource *because* of how they are connected to it?"

*   **Mechanism**: Permissions are defined by traversing a graph of relationships. For example, "A user can edit a document if they are in a group that owns the document's parent folder."
*   **Example**: Social networks ("You can see this post because you are a 'friend' of the author") or modern collaboration tools like Notion or Figma. Google's Zanzibar system is a famous large-scale implementation of ReBAC.
*   **Pros**: Extremely powerful and flexible for modeling complex, real-world permissions (e.g., organizations, hierarchies, social connections).
*   **Cons**: Can be complex to design and requires a data model and query engine capable of efficiently evaluating relationships.

## Authorization Models vs. Frameworks (The Role of OAuth 2.0)

It's essential to distinguish between an access control **model** and an authorization **framework** like [[oauth|OAuth 2.0]].

*   **Access Control Models (RBAC, ABAC, etc.)**: These are the *rules engines* or the logic that an application uses to make a final "permit" or "deny" decision. They answer the question: "Is this user allowed to perform this action on this resource?"
*   **Authorization Frameworks ([[oauth|OAuth 2.0]])**: These are protocols designed for **delegated authorization**. OAuth 2.0 doesn't define *how* an access decision is made. Instead, it provides a secure way for a user to grant a third-party application limited access to their resources without sharing their credentials.

### How They Work Together

In a modern system, these concepts are complementary:
1.  A client application uses the **[[oauth|OAuth 2.0]]** flow to request access to an API on behalf of a user.
2.  The user authenticates and consents, and the OAuth server issues an **Access Token**. This token represents the delegated permission. It may contain information like the user's ID and the approved **scopes** (e.g., `read:profile`, `write:calendar`).
3.  The client application sends the Access Token to the API with its request.
4.  The API's authorization middleware validates the token. It then uses the information within the token (user ID, scopes) as **input** to its internal **access control model** (e.g., RBAC or ReBAC).
5.  The access control model makes the final decision. For example: "The token is valid for user '123' with scope `write:calendar`. Does user '123' have the 'Editor' role (RBAC) required to modify this specific calendar event?"

In short, **OAuth 2.0 carries the authorization proof**, while **the access control model enforces the rules**.

## Related Concepts

*   [[authentication]]: The prerequisite for authorization.
*   [[oauth|OAuth 2.0]]: A framework for delegating authorization.
*   [[api-security|API Security]]: Broader security considerations for APIs.
*   [[owasp|OWASP]]: Provides guidelines on preventing authorization failures, such as Insecure Direct Object References (IDOR).

---

## Resources & Links

### Articles

1.  **[Authorization Methods Overview - Ping Identity](https://www.pingidentity.com/en/resources/identity-fundamentals/authorization/authorization-methods.html)**
    An excellent introductory article from Ping Identity that defines authorization and provides a clear, high-level overview of the primary access control models, including RBAC, PBAC, ABAC, and Privileged Access Management (PAM). It serves as a great starting point for comparing the different approaches.

2.  **[Deep Dive into Policy-Based Access Control (PBAC) - Heimdal Security](https://heimdalsecurity.com/blog/policy-based-access-control/)**
    This Heimdal Security blog post offers a focused look at Policy-Based Access Control (PBAC). It details how PBAC provides dynamic, fine-grained control by evaluating policies against a rich set of attributes, contrasting it with the more static nature of traditional RBAC.

3.  **[Understanding Attribute-Based Access Control (ABAC) - Okta](https://www.okta.com/en-gb/blog/identity-security/attribute-based-access-control-abac/)**
    From identity leader Okta, this article clearly explains the mechanics of Attribute-Based Access Control (ABAC). It provides practical examples of user, resource, and environmental attributes, illustrating how ABAC enables highly granular and context-aware authorization policies.

4.  **[Implementing Role-Based Access Control (RBAC) - Auth0](https://auth0.com/docs/manage-users/access-control/rbac)**
    Auth0's official documentation provides a practical guide to implementing Role-Based Access Control (RBAC). It covers the core concepts of assigning permissions to roles and roles to users, offering a foundational perspective from a major identity-as-a-service provider.

5.  **[What Is Discretionary Access Control (DAC)? - Built In](https://builtin.com/articles/discretionary-access-control)**
    An article from Built In that clearly defines Discretionary Access Control (DAC), a model where resource owners have the authority to grant or revoke access. It explains the pros and cons, highlighting its flexibility but also its challenges for centralized management in large-scale systems.

6.  **[Mandatory Access Control (MAC) Explained - TechTarget](https://www.techtarget.com/searchsecurity/definition/mandatory-access-control-MAC)**
    A concise definition from TechTarget explaining Mandatory Access Control (MAC). The article describes it as a non-discretionary model where a central authority enforces access based on security labels, making it suitable for high-security environments like military and government agencies.

7.  **[How to Implement Relationship-Based Access Control (ReBAC) - freeCodeCamp](https://www.freecodecamp.org/news/implement-relationship-based-access-control/)**
    This freeCodeCamp article provides a developer-oriented perspective on implementing Relationship-Based Access Control (ReBAC). It explains how permissions can be derived from the relationships between entities, offering a powerful model for applications with complex, graph-like permission structures.
