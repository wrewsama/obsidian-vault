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
    
---
Source: https://www.goodreads.com/book/show/27968891-site-reliability-engineering
