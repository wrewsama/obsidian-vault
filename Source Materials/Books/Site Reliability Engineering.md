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

---
Source: https://www.goodreads.com/book/show/27968891-site-reliability-engineering
