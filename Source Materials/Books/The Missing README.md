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

## Code Reviews
- receiving
    - fill out the request adequately
    - don't get attached to it
    - practice empathy, but don't tolerate rudeness
- giving
    - triage the requests (urgency, importance, complexity, size etc.)
    - understand the change first, then give comprehensive feedback
    - acknowledge the good points too
    - distinguish between issues, suggestions, and nitpicks (e.g. with a `nit: ` prefix)

## Software Delivery
- 4 phases: Build, Release, Deploy, Rollout
- build guidelines
    - use semver
    - package different resources separately
- release guidelines
    - keep them immutable (don't overwrite, release a new version instead)
    - release frequently with a transparent release schedule
    - use changelogs (for supporters) and release notes (for clients)
- deployment guidelines
    - automated
    - atomic (all or nothing)
    - independent (no dependencies on other deployments)
- rollout guidelines
    - monitor them
    - consider feature flags, circuit breakers, dark launches, canary deployments, and blue-green deployments

## Oncall
- communication
    - stay calm and polite
    - be concise
    - respond quickly
    - update periodically
- incident process
    - triage
    - coordinate (teams and customers)
    - mitigate
    - resolve
    - follow-up
- root cause analysis technique: the 5 whys: take the problem and ask why it happened, take the answer to that and ask why again, repeat until you have 5 whys
- avoid "firefighter heroics"

## Design Documents
- Process
    - Define the problem; Clarify with stakeholders
    - Do research
    - Conduct experiments and tech spikes
    - Give time to mull about it
- Template
    - Introduction (problem and proposed solution)
    - Current state / Context
    - Motivation for Change (business need)
    - Requirements
    - Potential solutions and evaluation
    - Proposed solution
        - system diagram
        - UI/UX changes with mockups
        - Code changes (high level)
        - API changes
        - Persistence layer changes (new storage technologies, new schemas, etc.)
    - Test plan
    - rollout plan
    - unresolved questions

## Creating Evolvable Architectures
- fundamental principles
    - Minimise complexity for software that needs to last. Achieve this through minimising dependency and obscurity
    - KISS
    - YAGNI
    - Principle of Least Astonishment: i.e. make features behave the way most users would expect it to
    - encapsulate each part of the domain
- API guidelines
    - keep them small
    - keep them forward (new client old server) and backward (old client new server) compatible
    - keep them versioned
- data guidelines
    - avoid shared DBs
    - use schemas and schema migration tools
    - keep them forward and backward compatible (same as APIs)

## Working With Managers
- 1:1s
    - topics: big picture, feedback, career advice / goals, personal issues
    - you set the agenda
- PPPs
    - essentially a status update
    - Progress, Plans, Problems
- feedback 
    - seek out and give feedback
    - use the SBI framework: Situation, Behaviour, Impact
- responses to when things aren't working (in order)
    - direct feedback to manager
    - feedback to skip manager
    - feedback to HR
    - (if nothing improves after 3 months) leave

## Career Advice
- Be T-Shaped
- Participate in Engineering Programs (interviewing, brown bags, conferences, reading groups, open source, etc.)
- Steer your promotions: understand the evaluation criteria, get specific feedback
- Change jobs when you have a solid reason to
- Pace yourself for a decades-long marathon

---
Source: https://www.goodreads.com/book/show/57271519-the-missing-readme
