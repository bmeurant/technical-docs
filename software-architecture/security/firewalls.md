---
title: Firewalls
tags:
  - security
  - networking
  - infrastructure
  - firewall
date: 2025-11-02
---
# Firewalls

A firewall is a network security device or software that monitors and controls incoming and outgoing network traffic based on predetermined security rules. It establishes a barrier between a trusted internal network and an untrusted external network, such as the Internet, and is a fundamental component of any layered security (defense-in-depth) strategy.

The primary purpose of a firewall is to allow legitimate traffic through while blocking unauthorized or malicious traffic.

## How Firewalls Work

Firewalls work by analyzing data packets and determining whether they should be allowed to pass through, based on a set of rules. These rules can be based on various criteria, including:

-   **Source/Destination IP Address**: Blocking traffic from known malicious IP addresses.
-   **Source/Destination Port**: Allowing traffic only on specific ports (e.g., allowing web traffic on port 443 but blocking Telnet on port 23).
-   **Protocol**: Filtering based on the network protocol being used (e.g., TCP, UDP, ICMP).
-   **Application Payload**: Inspecting the actual content of the packets for malicious signatures (in more advanced firewalls).

## Types of Firewalls

Firewalls have evolved significantly over the years, leading to several distinct types, each operating at a different layer of the OSI model.

### Packet-Filtering Firewalls

-   **OSI Layer**: Layer 3 (Network) and Layer 4 (Transport).
-   **Functionality**: This is the most basic type of firewall. It inspects packets individually and makes decisions based on IP addresses, protocols, and port numbers. It does not track the state of connections.
-   **Pros**: Fast and efficient.
-   **Cons**: Limited security as it doesn't understand the context of the traffic.

### Stateful Inspection Firewalls

-   **OSI Layer**: Primarily Layer 4 (Transport).
-   **Functionality**: Also known as dynamic packet-filtering firewalls, they maintain a state table of all active connections. This allows them to determine if an incoming packet is part of an established, legitimate conversation. For example, it will only allow a response from a server if it knows a request was first sent from an internal client.
-   **Pros**: Offers significantly better security than simple packet filtering.
-   **Cons**: Can be more resource-intensive.

### [[proxy-pattern|Proxy Firewalls]] (Application-Level Gateway)

-   **OSI Layer**: Layer 7 (Application).
-   **Functionality**: A [[proxy-pattern|proxy firewall]] acts as an intermediary for all communication between the client and the server. The client connects to the firewall, and the firewall establishes a separate connection to the destination server. This means there is no direct connection between the client and the server. It can inspect the application-layer payload (e.g., HTTP commands) to filter out malicious content.
-   **Pros**: High level of security and detailed logging.
-   **Cons**: Can introduce latency and may not be compatible with all protocols.

### Next-Generation Firewalls (NGFW)

-   **OSI Layer**: Spans multiple layers, up to Layer 7 (Application).
-   **Functionality**: NGFWs combine the features of traditional firewalls (packet filtering, stateful inspection) with more advanced security functions, such as:
    -   **Deep Packet Inspection (DPI)**: Examining the actual data content of a packet.
    -   **Intrusion Prevention Systems (IPS)**: Detecting and blocking known exploits.
    -   **Application Awareness**: Identifying and controlling traffic based on the specific application (e.g., blocking Facebook but allowing Salesforce).

### Web Application Firewalls (WAF)

-   **OSI Layer**: Layer 7 (Application).
-   **Functionality**: A WAF is a specialized type of firewall designed specifically to protect web applications. It sits in front of web servers and analyzes HTTP/HTTPS traffic, filtering and blocking malicious requests like SQL injection, Cross-Site Scripting (XSS), and file inclusion, which are often identified in the [[owasp|OWASP Top 10]].

## Related Concepts

-   [[gatekeeper|Gatekeeper Pattern]]: A WAF is a concrete implementation of the Gatekeeper pattern.
-   **Network Security Groups (NSGs) / Security Groups**: In cloud environments (like Azure and AWS), these act as virtual firewalls, allowing you to define inbound and outbound traffic rules for your virtual machines and subnets.

---

## Resources & Links

### Articles

1.  **[What Is Firewall Security? - F5](https://www.f5.com/glossary/firewall-security)**
    An article from F5 that defines a firewall as a critical component of network security that acts as a barrier between trusted and untrusted networks. It explains the different types of firewalls, from packet filtering to application-level gateways, and their role in a defense-in-depth security strategy.

2.  **[What is a Firewall? - Fortinet](https://www.fortinet.com/resources/cyberglossary/firewall)**
    A glossary entry from Fortinet that provides a clear and concise definition of a firewall. It covers the evolution of firewall technology, from simple packet filters to modern Next-Generation Firewalls (NGFWs) that incorporate advanced features like Intrusion Prevention Systems (IPS) and deep packet inspection.

3.  **[What is a next-generation firewall (NGFW)? - Cloudflare](https://www.cloudflare.com/learning/security/what-is-next-generation-firewall-ngfw/)**
    A detailed explanation from Cloudflare's learning center focused specifically on Next-Generation Firewalls (NGFWs). It contrasts NGFWs with traditional firewalls and details their advanced capabilities, such as application awareness, integrated intrusion prevention, and the ability to use external intelligence sources.
