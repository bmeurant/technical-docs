---
title: Fail-Fast
tags:
  - design-principle
  - debugging
  - robustness
date: 2025-10-02
---
# Fail-Fast

A **Fail-Fast** system is a principle that advocates for a system to report any condition that is likely to indicate a failure as soon as it is detected. Instead of attempting to continue execution with a potentially corrupt or invalid state, the system stops the current operation immediately, usually by throwing an exception.

---

## Core Idea & Goal

The primary goal of a fail-fast system is to make bugs **loud, obvious, and easy to debug**. A silent failure, where the system continues to run in a partially incorrect state, is the worst kind of failure, as it can lead to data corruption and make the original cause of the bug very difficult to trace.

Fail-fast is for detecting **bugs** and **internal contract violations**, not for handling predictable, external errors. For instance, invalid user input or a temporary network outage should be handled gracefully (a defensive programming technique), but a function receiving `null` from another part of your own system when it shouldn't is a bug that should trigger a fast failure.

**Example:**

```javascript
// This internal function requires a 'user' object with an 'id' to proceed.
function getInternalApiUrl(user) {
    // A missing user or id indicates a bug in the calling code.
    if (!user || !user.id) {
        // Fail-Fast: Stop everything. The developer needs to fix the calling code.
        throw new Error('Contract violation: getInternalApiUrl requires a user with an id.');
    }

    return `/api/users/${user.id}`;
}
```

---

## Benefits

-   **Rapid Bug Detection:** Errors are reported at the source, not somewhere downstream.
-   **Reduced Debugging Time:** The stack trace points directly to the cause of the problem.
-   **Increased Confidence & Reliability:** Prevents corrupted data and reduces the likelihood of unpredictable behavior.

---

## Relationship with Other Principles

-   **[[defensive-programming|Defensive Programming]]:** Fail-Fast is a key *strategy* within a defensive mindset. It is the strategy of choice when a defensive check reveals an impossible *internal* state that indicates a bug.

-   **[[design-by-contract|Design by Contract (DbC)]]:** DbC is the most formal and systematic way to implement a fail-fast system. A violation of a precondition, postcondition, or invariant is a contract violation that should immediately trigger a failure.
