Tags:
- [[Software Engineering]]
- [[Career Skills]]
---
## The Journey Ahead
- Core Competencies: Technical knowledge, Execution, Communication, Leadership
- The journey: Onboarding -> Ramping Up -> Contributing -> Operations -> Competence

## Getting to Conscious Competence
- Front-Load learning (during the first few months at a new job)
- learn by doing
    - ship simple code
    - do experiments on the code you own (e.g. test runs changing args and configs)
    - work on side projects
- read
    - internal: docs, code, tickets
    - external: books, papers, technical sites
- (sparingly) attend presentations & tech talks
- shadow and pair with seniors
- ask **good questions**
    - do your own research first (timebox it, know when to "give up" and get help)
    - when asking, say what you already know / already tried
    - don't interrupt others' focus, prefer async communication

## Working With Code
- Tech Debt: future work owed to fix shortcomings in existing code
- address tech debt with small refactors where possible, large refactors should be discussed and evaluated beforehand
- legacy code changes
    - identify change points and test points
    - break dependencies
    - write tests
    - make the refactor
- prefer "boring" technologies. Failure modes of boring technologies are well understood
- 2nd system syndrome: rewriting a system often ends up with an even more complex system

## Writing Operable Code
- Defensive programming
    - exceptions: throw as early as possible, catch only at the layer that can handle it
    - misc: immutability, avoid nulls, automated static checking, input validation, idempotence, retry backoff, resource cleanup
- log levels
    - TRACE: line-by-line, data structure dumps
    - DEBUG: useful for production debugging, but not normal operations
    - INFO: information about app state
    - WARN: potentially problematic situations
    - ERROR: problem that needs attention
    - FATAL: error that forces program to exit
- keep logs atomic, fast, and free from sensitive data
- metric measurements are cheap
    - measure resource pools, caches, data structures, data size, exceptions/errors, requests/responses, and CPU/IO intensive operations
- use call trace IDs on requests to create call traces
- config guidelines
    - prefer static configs and "boring" formats
    - log all non-secret configuration on start up
    - after start-up, validate the configuration
    - treat config as code, complete with version control
- when writing tools, prefer CLI-based for ease of scripting

## Managing Dependencies
- use semver
- avoiding dependency hell
    - if it's a simple dependency, consider just copying the code over to ensure stability
    - don't access transitive dependencies directly (similar to Law of Demeter)
    - pin versions
    - scope your dependencies (e.g. compile, runtime, test dependencies)

## Testing
- types of tests
    - unit: single behaviour
    - integration: multiple components
    - system: whole system
    - performance: can be load tests or stress tests
    - acceptance: performed by customer
- code quality checker tools: style, complexity, coverage
- prioritise what to test with a risk matrix: impact x likelihood of a failure mode
- how to achieve determinism in tests
    - seed RNGs
    - use dependency injection for non-deterministic dependencies (e.g. clocks, remote systems)
    - bind to port 0 (OS picks any available port)
    - clean up state
    - generate unique file / db paths
    - tests shouldn't depend on each other
---
Source: https://www.goodreads.com/book/show/57271519-the-missing-readme
