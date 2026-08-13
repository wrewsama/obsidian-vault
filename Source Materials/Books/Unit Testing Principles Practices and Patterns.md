Tags:
- [[Software Engineering]]
---
## Goal of Unit Testing
> Goal: enable _sustainable_ growth of the project
- code, including test code, is a liability, not an asset. Ensure the tests are good enough to pull their weight
- low coverage => not enough tests, but high coverage does not mean the code is well tested
- tests should
    - integrate into the dev cycle
    - target only important parts of the codebase
    - maximise value while minimising maintenance cost

## what is a unit test
- characteristics
    - verifies small piece of code
    - completes quickly
    - isolated
- SUT dependency types
    - shared: dependency that's shared between tests and can cause them to affect each other
    - private: not shared
    - out-of-process: dependency running outside the app (forms a Venn diagram with shared dependencies)
- integration tests: check if your code can _integrate_ with dependencies correctly

## anatomy of a unit test
- AAA pattern
    - Arrange: set state of the SUT and its dependencies
    - Act: call the method of the SUT you want to test
    - Assert: verify the result
- "smells"
    - multiple AAA sections
    - if statements
    - multiple actions in the Act section (indicates poor SUT interface)
- consider using 
    - reusable test fixtures for the Arrange section
    - parameterised tests when doing the same thing but with different input/output pairs
- test naming guidelines
    - no rigid policy
    - describe the scenario in _domain terms_, not based on implementation details

## pillars of good unit tests
- the pillars
    1. **protection against regressions**: based on the total amount of complexity and domain significance of the code that gets tested by the test
    2. **resistance to refactoring**: coupling to implementation details that cause tests to fail even when refactoring correctly
    3. **fast feedback**
    4. **maintainability**: difficulty of understanding and running the test
- the tradeoff
    - maintainability should be maximised
    - protection against regressions, resistance to refactoring, and fast feedback cannot all be maximised together, need trade-offs
    - resistance to refactoring should be maximised
    - need to strike a balance between protection against regressions and fast feedback
---
Source: https://www.goodreads.com/book/show/48927138-unit-testing
