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
- AAA pattern: Arrange, Act, Assert
- SUT dependency types
    - shared: dependency that's shared between tests and can cause them to affect each other
    - private: not shared
    - out-of-process: dependency running outside the app (forms a Venn diagram with shared dependencies)
- integration tests: check if your code can _integrate_ with dependencies correctly

## anatomy of a unit test
---
Source: https://www.goodreads.com/book/show/48927138-unit-testing
