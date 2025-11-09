---
title: API Compliance
tags:
  - api
  - compliance
  - security
  - gdpr
  - pci-dss
date: 2025-11-09
---

While data privacy and compliance are system-wide concerns, they have critical and specific implications for API design. As the primary interfaces for data exchange, APIs are a major focus for enforcing legal and industry regulations.

This document focuses on how standards like GDPR, PCI-DSS, PSD2 and others directly influence API architecture, security, and data handling practices. Understanding these requirements is essential for building secure, trustworthy, and legally sound APIs.

## Privacy by Design

Privacy by Design is a foundational concept stipulating that privacy should be integrated into the design and architecture of systems from the outset, not added as an afterthought.

Its seven core principles are:
1.  **Proactive not Reactive; Preventative not Remedial**: Anticipate and prevent privacy-invasive events before they happen.
2.  **Privacy as the Default Setting**: Personal data should be automatically protected in any given IT system. No action is required by the individual to protect their privacy.
3.  **Privacy Embedded into Design**: Privacy is an essential component of the core functionality, embedded into the system's design and architecture.
4.  **Full Functionality — Positive-Sum, not Zero-Sum**: Accommodate all legitimate interests and objectives in a positive-sum "win-win" manner.
5.  **End-to-End Security — Full Lifecycle Protection**: Ensure security from data creation to destruction.
6.  **Visibility and Transparency — Keep it Open**: Assure all stakeholders that the system operates according to the stated promises and objectives.
7.  **Respect for User Privacy — Keep it User-Centric**: Keep the interests of the individual user at the forefront.

---

## GDPR (General Data Protection Regulation)
*Reference: [GDPR Official Text](https://gdpr-info.eu/)*

The GDPR is a comprehensive data protection law in the European Union that governs the processing of personal data of EU residents. It sets strict rules for how data is collected, stored, used, and shared.

Key principles include data minimization, purpose limitation, and requiring a lawful basis for processing data, such as explicit user consent.

### API-Specific Considerations

APIs are a major channel for data processing, and GDPR compliance heavily influences their design.

*   **Consent Management**: APIs must provide endpoints for users to grant and revoke consent. Consent must be granular, allowing users to opt-in to specific data processing activities.
*   **Data Access & Portability**: APIs must be available for users to access their data (`Right to Access`) and export it in a machine-readable format (`Right to Data Portability`).

    ```mermaid
    sequenceDiagram
        participant User
        participant ClientApp as Client Application
        participant AuthServer as Authorization Server
        participant APIServer as API Server
        participant Database

        User->>ClientApp: Request my data
        ClientApp->>AuthServer: Authenticate User (e.g., via [[OAuth]])
        AuthServer-->>ClientApp: Access Token
        ClientApp->>APIServer: GET /api/v1/me/data (with Token)
        APIServer->>Database: Fetch user data
        Database-->>APIServer: User Data
        APIServer-->>ClientApp: { "user_data": "..." }
        ClientApp-->>User: Display/Download data
    ```
    *This diagram shows a typical flow for a user requesting their data via an API, a key requirement of GDPR.*

*   **Right to Erasure (`Right to be Forgotten`)**: APIs must expose endpoints (e.g., `DELETE /api/v1/me`) to allow users to request the deletion of their data. This action must be cascaded through all systems, including logs and backups, as part of a defined process.
*   **Data Minimization**: APIs should only expose the minimum amount of data necessary for a given function. Avoid returning large, monolithic data objects.
    *   For **[[GraphQL]]** APIs, this is a native feature, as clients must explicitly request the fields they need.
    *   For **[[REST]]** APIs, this can be achieved through several patterns:
        *   **Field Selection (Sparse Fieldsets)**: Allow clients to specify which fields to return via a query parameter, e.g., `GET /users/123?fields=id,name,email`.
        *   **Specialized Views**: Offer different "views" or "projections" of a resource. For example, a `GET /users/123?view=summary` could return a small subset of fields, while the default view returns a more complete object.
        *   **Dedicated Endpoints**: Create separate, specialized endpoints that return limited views of a resource, such as `/users/123/profile` for public information.
*   **Security**: All API endpoints must be secured using strong [[authentication]] and [[authorization]] mechanisms. Data in transit must be encrypted using [[SSL-TLS]].

---

## PCI-DSS (Payment Card Industry Data Security Standard)
*Reference: [PCI Security Standards Council](https://www.pcisecuritystandards.org/)*

PCI-DSS is a global standard for securing payment card data. Any organization that stores, processes, or transmits cardholder data must comply with its requirements.

The standard is built on 12 core requirements, including building and maintaining a secure network, protecting cardholder data, implementing strong access control measures, and regularly monitoring and testing networks.

### API-Specific Considerations

For APIs handling payment information, PCI-DSS compliance is non-negotiable.

*   **Scope Reduction**: The primary goal is to minimize the scope of PCI-DSS by avoiding direct contact with sensitive cardholder data.
    *   **Tokenization**: Use a third-party payment gateway to convert raw card details into a secure token. Your APIs then handle the token, not the Primary Account Number (PAN). This is an implementation of the [[valet-key|Valet Key]] pattern.
*   **Secure Transmission**: APIs must not transmit sensitive cardholder data over insecure channels. [[ssl-tls|SSL-TLS]] encryption is mandatory.
*   **No Sensitive Data Storage**: Never store sensitive authentication data (like CVC2 codes) after authorization. Your APIs should not log or store raw cardholder data.
*   **Access Control**: Implement robust [[authentication]] and [[authorization]] to ensure that only legitimate applications and users can access payment-related API endpoints.
*   **Vulnerability Management**: Regularly scan APIs for vulnerabilities, as required by PCI-DSS.

---

## PSD2 (Payment Services Directive 2)
*Reference: [European Commission on PSD2](https://finance.ec.europa.eu/regulation-and-supervision/financial-services-legislation/implementing-and-delegated-acts/payment-services-directive_en)*

PSD2 is a European regulation that mandates "Open Banking." It requires banks to grant licensed third-party payment service providers (PSPs) access to customer account data through secure, standardized APIs. This regulation is a prime example of compliance driving API development and innovation.

It defines two key types of third-party providers:
*   **AISP (Account Information Service Provider)**: Can access account data to provide services like account aggregation.
*   **PISP (Payment Initiation Service Provider)**: Can initiate payments from a user's account on their behalf.

### API-Specific Considerations

*   **Mandatory APIs**: Banks must build and expose highly secure and standardized APIs for account information (AIS) and payment initiation (PIS).
*   **Strong Customer Authentication (SCA)**: APIs must enforce SCA, a form of multi-factor authentication, for most payment actions and data access requests. This is often implemented using [[oauth|OAuth]] and [[openid-connect|OpenID Connect]] flows.
*   **Standardization**: While not a single standard, initiatives like the Berlin Group Standard have emerged to create common API specifications to fulfill PSD2 requirements.
*   **Consent Management**: APIs must provide mechanisms for customers to grant, manage, and revoke the consent given to third-party providers.

---

## Other Notable Regulations

*   **[HIPAA (Health Insurance Portability and Accountability Act)](https://www.hhs.gov/hipaa/index.html)**: A US law governing the privacy and security of protected health information (PHI).
    *   **API Implications**: APIs must enforce strict [[authorization]] to ensure "minimum necessary" access to PHI. All API requests and responses involving PHI must be logged for auditing purposes. Data in transit must be encrypted, and APIs should provide mechanisms for breach notifications.

*   **[HDS (Hébergement de Données de Santé)](https://esante.gouv.fr/labels-certifications/hds/certification-des-hebergeurs-de-donnees-de-sante)**: A French certification for entities hosting personal health data, based on ISO 27001.
    *   **API Implications**: While focused on the hosting environment, it imposes strict requirements on APIs, such as enforcing multi-factor authentication, detailed traceability for all data access, encryption of data at rest, and managing explicit user consent.

*   **[CCPA (California Consumer Privacy Act)](https://oag.ca.gov/privacy/ccpa)**: A California-specific data privacy law that grants consumers rights over their personal information.
    *   **API Implications**: APIs must provide endpoints to service consumer rights, such as `GET /consumers/me/data` for access and `DELETE /consumers/me/data` for deletion. An endpoint to manage the "Do Not Sell My Personal Information" preference is also a common requirement.

*   **[SOX (Sarbanes-Oxley Act)](https://www.congress.gov/bill/107th-congress/house-bill/3763)**: A US federal law enacted to protect investors from fraudulent financial reporting.
    *   **API Implications**: For APIs that expose financial data or trigger financial transactions, SOX requires strict access controls, change management, and extensive logging. Every API call that modifies financial data must be auditable to prove who made the change, what was changed, and when.

*   **[SecNumCloud](https://cyber.gouv.fr/secnumcloud-pour-les-fournisseurs-de-services-cloud)**: A security certification from the French National Cybersecurity Agency (ANSSI) for cloud providers.
    *   **API Implications**: While it applies to the cloud provider, using a SecNumCloud-certified platform means your APIs must adhere to a high standard of security, including a secure development lifecycle, robust authentication, and resilience against attacks as outlined by [[owasp|OWASP]].

*   **[PII (Personally Identifiable Information)](https://csrc.nist.gov/publications/detail/sp/800-122/final)**: This is a broad term for any data that can be used to identify a specific individual. Regulations like GDPR, HIPAA, and CCPA are all designed to protect different categories of PII.
    *   **API Implications**: When designing APIs, any data field that could be PII must be handled with care. This includes applying data minimization, securing endpoints, and ensuring that any PII is only exposed when strictly necessary and with the proper authorization.
