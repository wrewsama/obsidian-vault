Tags:
- [[SRE]]
---
# Introductory Material
---
## Intersection of Security and Reliability
- tradeoff: redundancy, fail-open vs fail-safe
- both are commonly sacrificed early in favour of faster feature development, but are expensive to fix later on

## Understanding Adversaries
- modelling insider risk
    - list possible actors, motives, actions, and target
    - mix and match permutations to get different scenarios
- designing for insider risk
    - least privilege
    - zero trust
    - multi-party authorisation
    - business justifications
    - auditing & detection
    - recoverability
# Designing Systems
---
## Safe Proxies
- simple way to improve security and reliability of existing products/tools
- clients access the tool through a proxy
- the proxy provides features like logging, authorisation (including multi-party auth), validation (to prevent human errors), etc.

## Design Tradeoffs
- feature development vs security/reliability 
- initial velocity vs sustained velocity
- invest in good planning and careful design to satisfy security, reliability, and functional requirements

## Design for Least Privilege
> Principle of Least Privilege: Users should have the minimum amount of access needed to accomplish their tasks.
- best practices
    - small, functional APIs (so you have give fine-grained access)
    - breakglass mechanisms (emergency access)
    - auditing
- when denying a user
    - if user has no privileges, avoid exposes too much info, just return 403
    - if user has some privileges, but not the requested one, consider providing a token that can be used to request access
- breakglass mechanisms
    - highly restricted (e.g. only SREs allowed, can only access from certain physical locations)
    - closely monitored and audited
    - regularly tested
- advanced controls: beyond yes/no restrictions
    - multi-party authorisation
    - 3 factor authorisation
    - temporary access
    - require business justification

## Design for Understandability
- relevance to security and reliability
    - easier to make bug-free changes => decreases likelihood of failures
    - more effective incident response
    - increased confidence in security assertions
- enemy of understandability is unmanaged complexity
    - architecture should break complexity down
    - security and reliability requirements should be centralised (e.g. auth and retries in the RPM service framework instead of each app)
- good practices
    - prefer narrow, obvious interfaces
    - interfaces should have a common object model
    - idempotence
    - identifiers should be human-readable and non-reusable
- Trusted Computing Base (TCB): minimal group of components that dictate the security of the system. Should be able to uphold the security policy regardless of the state of the outside world
- Security boundary: interface between TCB and the outside world
- need to separate system into components separated by security boundaries

## Design for a Changing Landscape
- need infrastructure / processes to support different types of reliability / security changes 
    - short term changes (e.g. fix for zero-day vulnerability)
    - medium term change (e.g. improvement to security posture)
    - long term change (e.g. large change (like migration to HTTPS) for regulatory reason)
- guidelines
    - keep dependencies up to date (e.g. by rebuilding frequently)
    - release frequently with automated testing
    - use containers to bundle dependencies with their apps (instead of the host OS)
    - use microservices

## Design for Resilience
- resilience: system's ability to delay and/or withstand breakage (not _recovery_)
- principles
    - each layer in the system should be independently resilient
    - figure out which features are prioritised (i.e. which have the highest cost of failures) beforehand so you know which can be throttled when issues arise
    - compartmentalise
    - add redundancy within the compartments
    - automate resilience measures
    - validate resilence regularly
- possible solutions (in order of consideration)
    1.  separate into failure domains (decrease blast radii)
    2. use high-availability services
    3. load-shedding (return error instead of serving request) or throttling (delay responses to postpone future requests)
    4. use low-dependency solutions (usually as a backup for emergency response)
---
Source: https://www.goodreads.com/book/show/52362720-building-secure-and-reliable-systems
