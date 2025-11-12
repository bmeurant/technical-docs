---
title: Test Doubles
tags:
  - software-engineering
  - testing
  - test-doubles
date: 2025-11-12
---

# Test Doubles: Stubs, Mocks, Fakes, Spies, and Drivers

In automated testing, a **Test Double** is a generic term for any object or component that "stands in" for a real production component during a test. Using test doubles is essential for achieving isolation, allowing you to test a unit of code without relying on its real dependencies. This makes tests faster, more reliable, and easier to debug.

The term was coined by Gerard Meszaros in his book *xUnit Test Patterns*. The main types of test doubles are Stubs, Mocks, Fakes, Spies, and Drivers.

---

## Types of Test Doubles

### Stub
A **Stub** provides pre-defined, "canned" answers to calls made during the test. It is primarily used to make the code under test run, but it doesn't assert anything about the interactions it receives.

- **Purpose**: To provide state.
- **Use Case**: When your test needs a dependency to return a specific value. For example, a stubbed repository method could return a specific user object without actually hitting the database.

### Mock
A **Mock** is an object that registers the calls it receives and can be programmed with expectations. At the end of the test, you can verify that the mock was called with the expected parameters.

- **Purpose**: To verify behavior.
- **Use Case**: When you need to check that your code correctly interacts with a dependency. For example, verifying that a logging service's `error` method was called exactly once with a specific message.

### Spy
A **Spy** is a "partial" mock. It wraps a real object, allowing it to behave as it normally would, but it also records the interactions it receives. This is useful when you want to verify a specific interaction without changing the object's overall behavior.

- **Purpose**: To observe and verify behavior on a real object.
- **Use Case**: When you want to test if a method was called on a real object, but you still need the real method to be executed.

### Fake
A **Fake** is an object that has a working implementation, but it is not the same as the production one. It often takes shortcuts and uses a simplified, lightweight implementation.

- **Purpose**: To provide a functional, but simplified, replacement for a complex dependency.
- **Use Case**: Using an in-memory database (like H2 or SQLite) instead of a full-fledged production database (like PostgreSQL) for faster integration tests.

### Driver
A **Driver** is a test double that simulates a high-level module calling a lower-level one. It is primarily used in the **bottom-up** integration testing approach. It provides the necessary inputs to the module under test and is generally simpler than the real high-level component it replaces.

- **Purpose**: To invoke and pass test data to a lower-level module.
- **Use Case**: If you are testing a data access layer (DAL) before the business logic layer is ready, a driver can simulate the business logic layer by making calls to the DAL.

---

## Summary and Relationship with Integration Strategies

The choice of test double often depends on the testing context, particularly in integration testing.

| Test Double | Primary Purpose                               | Commonly Used In                               |
|-------------|-----------------------------------------------|------------------------------------------------|
| **Stub**    | Provides a fixed response (state)             | Unit & Integration Testing (especially Top-Down) |
| **Mock**    | Verifies interactions (behavior)              | Unit & Integration Testing                     |
| **Spy**     | Records interactions with a real object       | Unit & Integration Testing                     |
| **Fake**    | Provides a lightweight implementation         | Integration Testing                            |
| **Driver**  | Invokes a lower-level module                  | Integration Testing (Bottom-Up)                |

### Test Doubles in Integration Testing Approaches

Here’s how stubs and drivers are specifically used in incremental integration testing:

| Approach              | Description                                                              | Test Double Used |
|-----------------------|--------------------------------------------------------------------------|------------------|
| **Top-Down**          | High-level modules are tested first. Lower-level modules are integrated progressively. | **Stubs** are used to simulate the missing lower-level modules. |
| **Bottom-Up**         | Low-level modules are tested first. Higher-level modules are integrated progressively. | **Drivers** are used to simulate the missing higher-level modules that call the modules under test. |
| **Hybrid (Sandwich)** | A mix of both. The middle layer is tested first, then testing proceeds upwards and downwards simultaneously. | Both **Stubs** and **Drivers** are used. |
