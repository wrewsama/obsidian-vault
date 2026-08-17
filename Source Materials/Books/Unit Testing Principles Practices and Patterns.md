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

## Mocks and Test Fragility
- test doubles: to fake dependencies of the SUT
    - mocks: to fake a command dependency
    - stubs: to fake a query dependency
- don't assert the _interaction_ with the stub, just set the fake result and move on
- observable behaviour (opposite of implementation details): either expose an _operation_ or a _state_ that helps a client achieve their goals
- hexagonal architecture: 2 layers, business logic inside, application services outside
    - application services orchestrate communication between the business logic and external dependencies
    - application services depend on business logic, not the other way around

## Unit Testing Styles
- Output based / State based / Communication based
- prefer output based, use state based if needed
- functional architecture: pure functional core with a mutable shell
    - core makes decisions
    - shell acts upon those decisions, causing the side effects
    - drawbacks
        - limited applicability to certain problems
        - worse performance and more code (extra indirection)

## Refactoring Towards Valuable Unit Tests
- 2 metrics for types of code
    - complexity & domain significance
    - number of collaborators (mutable or out of process dependencies)
- high complexity low collaborators: unit test heavily
- low complexity high collaborators: briefly integration test
- low complexity low collaborators: trivial, ignore
- high complexity high collaborators: refactor
    - split into high complexity part and high collaborator part, then treat those parts according to the above
- use the **humble object pattern**: split the hard-to-test dependency away from the logic
    - very simple when the business process is a single read-decide-write
    - for more complex processes, aim to split them into granular steps first, then apply the humble object pattern

## Why Integration Testing
- to verify the systems works correctly with out-of-process dependencies
- managed dependencies
    - out-of-process dependencies that you can control
    - use the real deal in the integration tests
- unmanaged dependencies
    - out-of-process dependencies that you don't control
    - forms part of the system's observable output
    - use interfaces + mocks
- best practices
    - make domain model boundaries explicit (i.e. keep domain logic separate)
    - reduce the number of layers of indirection in the system
    - eliminate circular dependencies

## Mocking Best Practices
- only mock in integration tests, not unit tests
- apply the mocks to the lowest level interface that you own
- verify the calls made to the mocks (no missing calls, no unexpected calls)

## Testing the Database
- prerequisites
    - keep reference data (external data the application reads but never writes to) and the schema in source control
    - use migrations to maintain the DB's state
    - ensure each dev has their own separate instance
- ensure each AAA section has their own transaction(s), don't let a transaction span across test sections
- clean up test data at the start of each test
- reuse patterns
    - Arrange: Object Mother pattern
    - Act: Decorators
    - Assert: Fluent interface
        - e.g. `deployment.exists().with_host("foo")`
- test writes heavily
- only test the most complex and/or domain-significant reads

## Anti-Patterns
- testing private methods directly (indicates missing abstraction)
- accessing private state (should test through the public interface i.e. observable behaviour)
- leaking domain knowledge (happens when the setup of the expected value does something based on the algorithm itself e.g. trying to test a shortest path algorithm and setting up the expected value by running Dijkstra's on the input graph. Should hardcode instead to not imply specific implementation)
- code pollution (code that only handles tests should not appear in the production part of the codebase)
- working with time (should use dependency injection)
---
Source: https://www.goodreads.com/book/show/48927138-unit-testing
