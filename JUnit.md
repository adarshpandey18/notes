# JUnit ☕

JUnit is a framework for writing and running tests in Java.
It provides annotations and assertions that help developers verify that their code behaves as expected.

---

## Testing VS Unit Testing

- ### Testing

  - Broad term covering various methods to verify software functionality.
  - Includes unit tests, integration tests, system tests, acceptance tests, and more.
  - Ensure software works correctly in different scenarios and meets user requirements.  
  
- ### Unit Testing

  - Specific type of testing focused on individual units or components of software.
  - Tests the smallest testable parts like functions, methods, or classes in isolation.
  - Verify the correctness of each unit independently from the rest of the system.

---

## Steps

1. Prepare *(Setup test environment, write test methods)*
2. Provider test inputs.
3. Run the test.
4. Provide the expected output.
5. Report test result

> Every unit test have to be prepared differently for different units and need to provide different input & expected output.  

> In place of manually testing units we can use JUnit we have to provide input & expected output.

---

## Maven for JUnit

1. Create Project
