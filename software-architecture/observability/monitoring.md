---
title: Monitoring
tags:
  - system-design
  - monitoring
  - observability
  - devops
date: 2025-10-26
---

# Monitoring

**Monitoring** is a fundamental pillar of [[observability]] and a critical practice in modern system operations. It is the process of systematically collecting, analyzing, and using data to track a system's health, performance, and availability over time. While often used interchangeably with observability, monitoring is more accurately the *act* of watching and measuring a system using a predefined set of metrics and logs.

In a distributed system, where complexity arises from numerous interacting components, monitoring provides the essential feedback loop needed to maintain reliability and efficiency. It allows teams to move from a reactive state (fixing problems after they occur) to a proactive one (identifying and addressing issues before they impact users).

The core feedback loop of monitoring can be illustrated as follows:

```mermaid
graph TD
    subgraph System
        A[Application / Infrastructure]
    end

    subgraph Monitoring Pipeline
        B[1. Instrumentation] --> C[2. Collection & Aggregation];
        C --> D[3. Analysis & Visualization];
        D --> E[4. Alerting & Action];
    end

    A -- Telemetry Data --> B;
    E -- Feedback / Remediation --> A;

    linkStyle 0 stroke-width:2px,fill:none,stroke:gray;
    linkStyle 1 stroke-width:2px,fill:none,stroke:gray;
    linkStyle 2 stroke-width:2px,fill:none,stroke:gray;
    linkStyle 3 stroke-width:2px,fill:none,stroke:gray;
    linkStyle 4 stroke-width:2px,fill:none,stroke:blue,stroke-dasharray: 5 5;
```
*A diagram illustrating the continuous loop of a monitoring system, from data collection to action.*

---

## Aspects of a Monitoring System

```mermaid
graph TD
    A[Aspects of a Monitoring System] --> B(What to Monitor);
    A --> C(How to Enable Monitoring);
    A --> D(How to Use Monitoring Data);

    B --> B1(Health Monitoring);
    B --> B2(Availability Monitoring);
    B --> B3(Performance Monitoring);
    B --> B4(Security Monitoring);
    B --> B5(Usage Monitoring);

    C --> C1(Instrumentation);

    D --> D1(Visualization & Alerts);
```

A comprehensive monitoring strategy involves several distinct, yet interconnected, aspects that provide a holistic view of the system.

### Health Monitoring

Health monitoring provides an immediate, real-time snapshot of a system's operational status. Its primary goal is to answer the question: "Is the system running and able to process requests right now?"

This is typically implemented via **health check endpoints** (e.g., `/health`, `/status`, `/ping`) that services expose. These endpoints are consumed by other systems, such as a [[load-balancing|load balancer]] or a [[service-discovery]] registry, to make automated decisions.

For example, a load balancer will stop routing traffic to an instance that reports an "unhealthy" status, preventing user-facing errors.

**Example: Simple Health Check in Node.js/Express**
```javascript
app.get('/health', (req, res) => {
  // Check critical dependencies, like a database connection
  const isDbConnected = checkDatabaseConnection();

  if (isDbConnected) {
    res.status(200).json({ status: 'UP' });
  } else {
    res.status(503).json({ status: 'DOWN', reason: 'Database unavailable' });
  }
});
```

### Availability Monitoring

While health monitoring is instantaneous, **availability monitoring** tracks a system's operational uptime over a period. It answers the question: "Has the system been running reliably over the last day, week, or month?"

Availability is a core component of [[software-architecture/observability/index#SLIs, SLOs, and SLAs: The Business Impact of Observability|Service Level Agreements (SLAs)]] and is measured as a percentage of uptime (e.g., 99.9%, or "three nines"). For more details on how this is measured, see the fundamentals of [[availability]]. This involves tracking not just complete outages but also periods of degradation where the system is technically "healthy" but unusable. This data is crucial for capacity planning and ensuring reliability promises to customers are met.

See also: [[availability-patterns]].

### Performance Monitoring

Performance monitoring focuses on the efficiency, responsiveness, and [[workload-management#Compute Resource Consolidation|resource utilization]] of a system under load. It helps identify bottlenecks, degradation, and inefficiencies before they lead to outages. Key metrics often align with the **Four Golden Signals**:

1.  **[[software-architecture/system-design-fundamentals/index#Latency vs. Throughput|Latency]]**: The time it takes to service a request.
2.  **Traffic**: The demand on the system (e.g., requests per second).
3.  **Errors**: The rate of requests that fail.
4.  **Saturation**: How "full" the service is (e.g., CPU utilization, memory usage).

By monitoring these metrics, teams can detect when a [[performance-antipatterns|performance antipattern]] is emerging and take corrective action, such as [[software-architecture/system-design-fundamentals/index#Scalability|scaling resources]], [[rdbms#SQL Tuning|optimizing queries]], or leveraging a [[caching]] or [[cdn|CDN]] strategy.

### Security Monitoring

Security monitoring is dedicated to detecting, analyzing, and responding to potential security threats. It involves collecting and correlating data from various sources (firewalls, application logs, identity providers) to identify malicious activity.

Common goals include:
- **Detecting unauthorized access**: Monitoring failed and successful login attempts to spot brute-force attacks.
- **Identifying intrusion attempts**: Analyzing network traffic for patterns indicative of a DDoS attack or vulnerability scanning.
- **Auditing data access**: Recording which users access sensitive data to ensure compliance and detect insider threats.

This often involves specialized tools like Security Information and Event Management (SIEM) systems that aggregate and analyze security-related events from across the entire IT infrastructure.

### Usage Monitoring

Usage monitoring tracks how users interact with an application. Unlike performance monitoring, which focuses on system behavior, usage monitoring focuses on user behavior. The insights gathered are valuable for:

- **Business Intelligence**: Identifying popular or underutilized features to guide product development.
- **Capacity Planning**: Using trends in user activity (e.g., number of transactions) to forecast future resource needs.
- **Detecting User Friction**: A high abandonment rate on a checkout page might indicate a bug or a poor user experience.
- **Billing and Quotas**: In multi-tenant systems, usage data is essential for charging customers based on consumption and enforcing resource limits.

### Instrumentation

Instrumentation is the **how** of monitoring. It is the process of integrating code into an application to generate the telemetry data (metrics, logs, and traces) that monitoring systems consume. Meaningful monitoring is impossible without effective instrumentation. In cloud-native environments, this is often handled by deploying collection agents using the [[sidecar]] pattern.

#### The Rise of OpenTelemetry
Historically, instrumentation was often done using vendor-specific agents or libraries, leading to vendor lock-in. **[[opentelemetry|OpenTelemetry]] (OTel)** emerged as a CNCF standard to solve this problem by providing a unified framework for all telemetry data.

The promise of OpenTelemetry is three-fold:

1.  **Vendor-Neutral Instrumentation**: Instrument your code once and send telemetry data to *any* OTel-compatible backend (open-source or commercial).
2.  **Unified Telemetry**: OTel provides a single standard for metrics, traces, and logs, allowing for powerful correlation between them.
3.  **Automatic Instrumentation**: For many popular frameworks, OTel offers auto-instrumentation libraries that generate valuable telemetry with no manual code changes.

For a detailed explanation of the standard, its components, and its data models, see the main **[[opentelemetry]]** page.

### Visualization and Alerts

This is the **action** phase of monitoring, where raw data is transformed into actionable insights.

-   **Visualization**: Involves presenting data in human-readable formats like dashboards, graphs, and heatmaps. Tools like Grafana or Kibana allow operators to explore data, spot trends, and correlate events across different services. A well-designed dashboard can instantly reveal the health of a complex system.

-   **Alerting**: Proactively notifies operators when a system metric crosses a predefined threshold, indicating a potential problem. A robust alerting strategy is crucial for minimizing downtime.

The alerting pipeline typically follows this flow:

```mermaid
graph TD
    A[Metric Captured] --> B{Threshold Rule};
    B -- Breached --> C[Alert Fired];
    B -- Normal --> D[No Action];
    C --> E[Notification Sent];
    E --> F[Operator Acknowledges];

    subgraph Channels
        E --> G[Email];
        E --> H[Slack];
        E --> I[PagerDuty];
    end
```
*A diagram illustrating the alerting pipeline. A captured metric is evaluated against a predefined threshold. If the threshold is breached, an alert is fired, which in turn triggers a notification to be sent through various channels (like Email, Slack, or PagerDuty) to an operator, who can then acknowledge it.*

A key challenge is to make alerts meaningful and actionable, avoiding "alert fatigue" where operators become desensitized due to excessive, low-priority notifications.

---

## Resources & links

### Articles

1.  **[Monitoring and diagnostics guidance - Azure Architecture Center](https://learn.microsoft.com/en-us/azure/architecture/best-practices/monitoring)**
    A comprehensive guide from Microsoft on best practices for monitoring cloud applications, covering data collection, health monitoring, and alerting.

2.  **[Application Monitoring Best Practices - IBM](https://www.ibm.com/think/topics/application-monitoring-best-practices)**
    An article from IBM detailing key practices for effective application monitoring, focusing on user experience, infrastructure, and setting up the right alerts.
