Tags:
- [[SRE]]
---
## Intro
- how Google hires SREs: 
    - standard SWE level
    - 85-99% of the required SWE level + relevant but rare skills
        - UNIX internals
        - networking (lower layers)
- at most half ops; at least half development
- purpose of SREs
    - maximise change velocity within the error budget
    - monitoring software that interprets data and alerts humans only when action is needed
    - emergency responses, automated as much as possible
    - preventing and addressing errors due to new changes
    - demand forecasting and provisioning resources

## Embracing Risk
- Issues with _extreme_ reliability
    - stymies feature development
        - more testing
        - more time investment on fault tolerance
        - more canary deployments
    - UX may be dominated by less reliable components anyway (e.g. client device, cellular network)
    - extra hardware for redundancy
- Approach to error budgeting
    - define Service Level Objective for uptime in a time period
    - measure actual uptime
    - remaining error budget can be calculated from that

## SLOs
- Service Level Indicators: quantitative measure of level service provided
    - usually aggregated, but as a distribution, not just average
    - may want to see instantaneous peak, or 99th percentile
- Service Level Objectives: Target values for the SLIs
    - focus on what users care about, then try to approximate it with what you can measure - not the other way around
    - overachieving can result in users becoming over-dependent on your system which is counterproductive. It may even be worth giving planned outages to prevent that (e.g. Google Chubby service)
- Service Level Agreements: Contract with users detailing the effects of meeting and missing certain SLOs

## Toil
- Toil = work that is:
    - manual
    - repetitive
    - automatable
    - reactive instead of proactive
    - no lasting impact on the product
    - scales linearly with service size
- < 50% of work should be toil, the other >= 50% should be development work that will
    - reduce toil directly
    - improves reliability, performance, or utilisation (reduces toil indirectly)

## Monitoring
- Getting, transforming, and showing quantitative data about a system
- can be:
    - black box: measure from a user's perspective; Tells you what is happening
    - white box: measure internal parts of the system; Tells you why certain things are happening
- A simple mix of metric collection, alerting, and dashboards works well
- Measure distribution of metrics (e.g. p99, p99.9), not just average
- 4 golden signals to monitor
    - latency
    - traffic
    - errors
    - resource usage
## Automation
- benefits
    - consistent
    - extensible
    - fast
- how automation evolves
    - manual operation
    - SRE has a local script
    - add support to a generic script everyone uses
    - the product ships with its own script
    - the product handles the issue automatically
- case study: Prodtest
    - manual cluster configuration -> automatic checking with Python unit tests -> automatic fixes with the same Python code

## Release Engineering
- SREs need to ensure binaries and configs can be built automatically and in a reproducible way
- Methods of config management
    - mainline: app applies the latest config
    - same package: configs and binaries in the same package
    - config packages: configs packaged into config packages and released alongside binary packages

## Simplicity
- Simplicity is necessary to achieve reliability
- characteristics of simple systems
    - no surprises in production
    - no software bloat - remove as much code as possible
        - actually delete, not just comment out / keep behind a feature flag
    - minimalist APIs - as few methods/endpoints as possible
    - encapsulation - components' parts should all be closely related, no unrelated parts

## Service Reliability Hierarchy of Needs
(from most fundamental to most advanced)
- monitoring
- incident response
- postmortems / RCAs
- testing
- capacity planning
    - resource estimation and allocation
- development (by SREs)
    - of tech for reliability
- a reliable product

## Alerting
- Monitoring system periodically
    - makes requests to a predefined endpoint to pull metric data
    - store in a time series
- Counters are preferred over gauges as gauges can miss important behaviours that happen in-between sampling intervals
- The time time series data can be aggregated arithmetically
- Alerting rules are set on that data, firing alarms once met

## Oncall
- Respond to outages, triage the problems and resolve them
- important on-call resources
    - clear escalation paths
    - well defined incident management procedures
    - blameless postmortem culture
- operational overload
    - too many alerts
    - distracts from the more serious alerts
- operational underload
    - not enough confidence in dealing with prod issues

## Troubleshooting
- Triage: determine the severity and appropriate response
    - first priority: make system work as "well" as possible
    - immediate response may not be a fix
- Examine: check if each component is behaving as expected
    - logging
    - metrics
- Diagnose: come up with hypotheses about the root cause
    - connections between components
    - _what_ is happening, _why_ and _where_ is it happening
    - what touched it last (recent code / config change etc)
- Test: Determine if the hypothesis is true
    - test in decreasing order of likelihood
- Treat the issue

## Learning From Emergency Response
- Document happenings and learnings from emergencies
- Ask "what if" questions, no matter how improbable they seem
- Test failures proactively

## Postmortems
- includes
    - incident timeline
    - response
    - root causes
    - follow-up steps taken to prevent incident from happening again
- ensures incident is recorded
- improves understanding of the root cause
- ensures steps were taken to reduce both the probability and the severity of similar issues

## Tracking Outages
- Big outages will have postmortems, but smaller ones need to be tracked too
- track alerts, acknowledgements, notifications

## Testing for Reliability
- 2 main types
    - traditional: evaluate correctness of software during development
    - production: evaluate correctness of software already in live production
- Traditional tests
    - unit
    - integration
    - system
        - smoke: simple but critical behaviour
        - performance: latency and resource usage
        - regression: ensuring historical bugs don't return
- production tests
    - config: check if prod's config is the same as the current config
    - stress: find limits of the system and components
    - canary: user testing on subset of servers

## SWE in SRE
- goal:
    - maintain uptime
    - keep low latency

## Load Balancing
- Between datacenters
    - DNS
    - Let multiple servers share a Virtual IP and put a network load balancer in front
- Within a datacenter, between backend servers
    - Limit total number of connections by subsetting
        - each client's requests get balanced between a subset of all the available servers
        - prevents client from needing to set up too many long-lived connections
    - policies
        - round robin within the client's subset
            - issue: unequal distribution due to different times taken to process different requests
        - round robin within the servers in the client's subset with the least number of active requests
            - issue: number of active requests doesn't capture the capability of a server if the requests are IO bound and the server has a CPU capable of handling more
        - weighted round robin within the client's subset

## Handling Overload
- rate limiting
    - Per-customer limits
    - Client side (frontend) throttling
        - based on:
            - requests: # of reqs attempted
            - accepts: # of reqs accepted by backend
- Other signals
    - Criticality: how important each request is, included in metadata of request
    - Utilisation: Resources needed by the request
- error handling
    - bubble up to client
    - retry
        - per-request limit: 3 tries
        - per-client limit: 10% of total request volume can be retries

## Cascading Failures
- cause
    - server overload
    - due to resource exhaustion
    - results in service unavailability
    - traffic gets routed to another server
    - that one gets overloaded and the cycle continues
- prevention
    - set up tooling to let server detect overload and respond accordingly:
        - serve degraded results
        - drop requests
    - queue requests and let servers pull them
    - avoid spamming retries
        - set limits
        - don't retry permanent errors - set error codes and handle accordingly
- cure
    - increase resources
    - stop healthchecks - these can increase load unecessarily (we know the servers are dead anyway)
    - restart
    - traffic control
        - drop fraction of all requests
        - degraded results
        - drop bad traffic
        - drop unimportant requests

## Distributed Consensus
- Paxos: distributed algo to make all nodes in a group agree on a single value
    - proposer sends a globally unique seq number to acceptors
    - acceptors will send an accept message to the proposer if they haven't seen another proposal with a higher number
    - if proposer receives acceptance from majority of acceptors, send commit message with the desired value
- common higher level abstractions
    - reliable replicated state machines: execute same set of ops in the same order
    - leader election
    - distributed coordination / locking
    - distributed message queues
- performance improvements
    - multi-paxos
    - fast paxos
    - quorum leases: temporarily pick a subset of replicas for a subset of data. Use that for read and write quorums

---
Source: https://www.goodreads.com/book/show/27968891-site-reliability-engineering
