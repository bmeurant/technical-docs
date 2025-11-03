---
title: Content Security Policy (CSP)
tags:
  - security
  - web-security
  - csp
  - browser-security
  - xss
date: 2025-11-02
---
# Content Security Policy (CSP)

Content Security Policy (CSP) is a browser security feature and an added layer of defense that helps detect and mitigate certain types of attacks, with a primary focus on **Cross-Site Scripting (XSS)** and data injection attacks. It is a standard that allows web administrators to control the resources (such as scripts, stylesheets, images, etc.) that a user agent (browser) is allowed to load for a given page.

By specifying a whitelist of trusted content sources, CSP makes it much harder for an attacker to execute malicious scripts on your web page.

## How CSP Works

A web server implements CSP by including a special HTTP response header: `Content-Security-Policy`. The value of this header is a string containing the policy directives.

Each directive defines the allowed sources for a specific type of resource.

**Example Policy:**

```http
Content-Security-Policy: default-src 'self'; script-src 'self' https://apis.google.com; img-src *; media-src media1.com media2.com; style-src 'self' 'unsafe-inline';
```

This policy specifies the following rules:
-   `default-src 'self'`: By default, resources can only be loaded from the page's own origin.
-   `script-src 'self' https://apis.google.com`: Scripts can only be loaded from the page's origin and from `https://apis.google.com`.
-   `img-src *`: Images can be loaded from any origin (wildcard).
-   `media-src media1.com media2.com`: Audio and video files can only be loaded from `media1.com` and `media2.com`.
-   `style-src 'self' 'unsafe-inline'`: Stylesheets can be loaded from the page's origin, and inline styles (`<style>` blocks and `style` attributes) are permitted.

When a browser receives this header, it will enforce the policy and block any resources that do not come from the whitelisted sources.

## Common CSP Directives

-   `default-src`: A fallback for other fetch directives if they are not specified.
-   `script-src`: Defines valid sources for JavaScript.
-   `style-src`: Defines valid sources for stylesheets (CSS).
-   `img-src`: Defines valid sources for images.
-   `font-src`: Defines valid sources for fonts.
-   `connect-src`: Restricts the URLs which can be loaded using script interfaces (e.g., `fetch`, `XMLHttpRequest`). This is particularly important for securing API calls.
-   `frame-src`: Defines valid sources for nested browsing contexts like `<iframe>`.
-   `object-src`: Restricts the sources for the `<object>`, `<embed>`, and `<applet>` elements.
-   `report-uri` / `report-to`: Instructs the browser to POST reports of policy violations to a specified URL. This is invaluable for monitoring and refining a CSP.

## The Danger of `'unsafe-inline'` and `'unsafe-eval'`

CSP's primary goal is to eliminate XSS vulnerabilities, which often rely on injecting inline scripts (e.g., `<script>alert(1)</script>`) or using string evaluation functions like `eval()`.

-   `'unsafe-inline'`: Allows the use of inline `<script>` elements, inline event handlers (like `onclick`), and `javascript:` URIs. Using it significantly weakens the policy.
-   `'unsafe-eval'`: Allows the use of `eval()` and similar methods for creating code from strings.

In modern web development, it is a best practice to avoid these keywords. Instead, scripts should be loaded from external files, and inline event handlers should be replaced with `addEventListener` calls.

## Reporting Violations

CSP is not just a blocking mechanism; it is also a reporting tool. By using the `report-uri` or `report-to` directive, you can instruct the browser to send a JSON report to a specified endpoint whenever a policy violation occurs. This allows developers to monitor for potential attacks or identify misconfigurations in their policy.

**Example with Reporting:**
```http
Content-Security-Policy: default-src 'self'; report-uri /csp-violation-report-endpoint;
```

## Security Benefits

-   **XSS Mitigation**: This is the primary benefit. By restricting where scripts can be loaded from, CSP makes it extremely difficult for an attacker to inject and execute a malicious script.
-   **Clickjacking Protection**: Using the `frame-ancestors` directive, you can control which sites are allowed to embed your page in an `<iframe>`, preventing clickjacking attacks.
-   **Data Exfiltration Prevention**: By restricting `connect-src`, `form-action`, etc., you can control where data can be sent from your page, making it harder for attackers to steal sensitive information.

## Related Concepts

-   [[owasp|OWASP]]: CSP is a key defense against many of the vulnerabilities listed in the OWASP Top 10, especially Cross-Site Scripting.
-   **Cross-Site Scripting (XSS)**: The main type of attack that CSP is designed to prevent.

---

## Resources & Links

### Articles

1.  **[Content Security Policy (CSP) - MDN Web Docs](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CSP)**
    The definitive technical guide from Mozilla Developer Network. It provides a comprehensive overview of CSP, detailing all the fetch directives, document directives, and navigation directives. It's an essential resource for understanding the nuances of constructing a robust and effective policy.

2.  **[Content Security Policy - web.dev](https://web.dev/articles/csp)**
    A practical guide from Google's web.dev team that explains how to use CSP to mitigate common vulnerabilities like Cross-Site Scripting (XSS). It provides clear examples and best practices for implementing a strict CSP, emphasizing the move away from `unsafe-inline` and the adoption of modern, nonce- or hash-based approaches.
