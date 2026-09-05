Tags:
- [[SRE]]
---
## How SRE Relates to DevOps
- DevOps is a set of high-level guidelines
- SRE is a specific role, with specific practices that also adhere to the DevOps guidelines, e.g.
    - treating ops as a software problem
    - managing SLOs
    - minimising toil / automation
    - reducing cost of failure
    - share ownership and tooling with product teams
## Implementing SLOs
- set up an SLI, preferably as the ratio of 2 numbers (e.g. ratio of successful responses to total responses)
    - set SLIs for different parts of the system based on their type (request-driven, pipeline, storage)
- implement the SLIs (i.e. set up the instrumentation and analysis tools to calculate the SLI values)
- calculate starter SLOs from SLI results
    - in general, stats over a 4 week rolling window are recommended
- get stakeholder agreement for the SLOs
- establish error budget policy (what to do when the error budget runs out) and get stakeholder agreement
- document SLOs and error budget policy
- continuously improve SLO targets: plot error budget loss against actual outages (e.g. through support ticket count) and adjust SLOs to better measure the customer dissatisfaction 
- SLOs and error budget loss can then be used to make decisions (e.g. prioritise reliability work)

## SLO Case Studies
- perfect is the enemy of good, start with "good" SLOs and iterate from there
- VALET SLOs: Volume, Availability, Latency, Errors, Tickets

## Monitoring
- desirable characteristics for the monitoring system
    - freshness of data: ensure that corresponding metric changes can be observed soon after incidents occur
    - support for calculations: need sufficient granularity and retention period, need to support statistical functions
    - good interfaces (dashboards) for different audiences
    - alerts: classifiable and suppressable
- sources of monitoring data
    - metrics
    - logs
- best practices for managing the monitoring system
    - config as code
    - consistency
    - loose coupling within components of the monitoring system (e.g. collection/storage/alerting/dashboards etc.)
- metrics to monitor
    - changes
    - dependencies
    - resource saturation (e.g. RAM, disk, CPU, etc.)
    - traffic status (e.g. HTTP 4XX, 2XX etc.)
    - in general, ensure every metric serves a purpose (i.e. they should make issue detection/diagnosis faster in some way)
## Alerting on SLOs
- alerting rule considerations
    - precision
    - recall
    - detection time: time taken between the issue occurring and the alert being sent
    - reset time: how long the alerts continue to fire after the issue is solved
- recommended alert rule: **Multi-Window, Multi-Burn-Rate Alerts**
    - burn rate: how fast the service is consuming the error budget, i.e. `error_rate / error_budget`
    - multi-burn-rate: different alerting rules for different burn rates (e.g. ticket on burn rate = 1, pager on burn rate = 6)
    - multi-window: alert only when the burn rate exceeds the threshold for both a short window and a long window (set short window to be 1/12 of the long window)
        - short window: fast reset but low precision (many false positive alerts)
        - long window: slow reset but high precision
        - together: fast reset and high precision
- edge cases
    - low traffic services: any alert would be a huge chunk of the error budget
        - generate artificial traffic
        - combine smaller services
        - modify the product to minimise the significance of a single failure / incident (e.g. add retry to client)
    - extreme high/low availability goals: require parameter tuning or system changes (e.g. canary rollouts)
- scalability: group request types and reuse the alerting parameters for different requests in the same group

## Eliminating Toil
- common types
    - business processes
    - production interrupts
    - release shepherding
    - migrations
    - capacity planning
- management strategies
    - identify and measure (e.g. man-hours spent)
    - eliminate the root cause by modifying the system
    - reject the toil altogether, you can use SLOs to drive this decision
    - self-service methods for users
    - automation
        - start with partial automation: a well defined interface where some operations can still be performed by humans
        - automate high priority operations first, incrementally increase automation
        - iterate based on feedback
     - assess the risk of automation, use defensive software wherever possible
     - promote toil reduction to management and colleagues
     - consider open source / 3rd party tools
## Simplicity
- **practical** measures of complexity
    - time taken to explain something to a new joiner
    - time taken to training a new joiner to go oncall
    - administrative diversity: number of says to configure similar setttings
    - number of unique configurations deployed
    - age of the system
- how to regain simplicity
    - remove dependencies
    - remove duplicated calls
    - identify request amplification (i.e. several levels of retries)
    - redesign to break cyclic dependencies

## On-Call
- new team onboarding
    - documentation/code deep dives, codelabs, read handoffs and postmortems, disaster role-playing, playbooks, shadowing
- pager load management
    - target: maximum 2 incident alerts per 12-hour shift
    - ensure sufficient testing before releases to minimise bugs (to in turn minimise pages) 
    - ensure identification and mitigation can take place before the page triggers again
    - alerts should ALL be immediately actionable
    - test new alerts thoroughly
        - deploy to production, but don't send the page
        - keep it in that mode for long enough to experience periodic production conditions (e.g. weekends, rollouts, etc.)
    - follow-up and identify the root cause of every page
    - remain vigilant of the state of on-call
- on-call schedule flexibility: automate scheduling, allow short-term swaps
- on-call team dynamics: empower ops engineers with SRE principles, improve team relations

## Incident Management
- Incident Command System
    - goals: Coordinate, Communicate, Control
    - roles: Incident Commander, Communications lead, Ops Lead
- preparation
    - decide on a communication channel
    - prepare contacts beforehand so you can keep them in the loop when incidents occur
    - establish the criteria for an incident
- Drills e.g. Disaster Recovery Testing, Disaster Role playing

## Postmortem Culture
- desirable characteristics of a postmortem
    - clear, concise, and well-organised
    - concrete action items
    - blamelessness
    - good depth and breadth of impact / flaws across multiple systems
    - promptness
- tooling
    - creation (automatically push metadata along with postmortem creation)
    - [checklist](https://drive.google.com/drive/folders/1t7fO8M3EZFeuu4GmzvStd0TGDI4bDCeb) ensure postmortem has all the required content
    - storage, analysis, and follow-up on postmortem action items

## Managing Load
- ensure unhealthy machines don't count towards the autoscaler's utilisation average
- consider _vertical autoscaling_ in stateful systems
    - quite literally automatic vertical scaling, works with containers / VMs
- keep sufficient buffer in the autoscaling policy to keep services far a way from system bottlenecks (e.g. cpu)
- set constraints to prevent autoscaling from going out of control. Consider the downstream services too
- add kill switches and manual overrides
- autoscale before load shedding

## Non-Abstract Large System Design (NALSD)
- start with a basic design that works in principle
    - is it possible to satisfy the requirements (ignoring capacity e.g. cpu, ram)
    - can we do better (faster, smaller, more efficient)
- scale up
    - is it feasible (can it meet the required scale)
    - is it resilient (can it fail gracefully and handle component / datacenter failures)
    - can we do better (again)

## Data Processing Pipelines
- best practices
    - define SLOs (e.g. freshness, correctness)
    - account for dependency SLAs
    - documentation
        - system design
        - BAU processes and playbooks
    - development lifecycle
        - prototyping -> dry-run testing (nonprod env with 1% of prod data) -> staging -> canary -> rolling deployment to prod
    - identify hotspots (resources under high contention) and break them down into fine-grained pieces
    - use autoscaling and resource planning
    - security policies (e.g. TTLs for PII, access control)
    - plan escalation paths
- pipeline maturity matrix
    - failure tolerance
    - scalability
    - monitoring and debugging
    - transparency / ease of implementation
    - unit and integration testing

## Config Design and Best Practices
- config philosophy
    - KISS, minimise required configs
    - config options should ask the user the questions required to help them achieve their business goals
    - we can still support power users by providing optional configs, with defaults (can be static or dynamic) if they aren't specified
- config implementation
    - separate config and data (i.e. have a decoupled config interface that generates the config data ingested by the system)
    - tooling: validation, linting, version control
    - deployments
        - should support: gradual rollout, rollbacks (manual and auto)

## Config Specifics
- goal: reduce config-induced toil, usually from managing large numbers of highly duplicated config files
- option 1: remove configs or replace them with dynamic defaults (best option, if possible)
- option 2: set up a config system (see previous chapter for implementation guidelines)

## Canarying Releases
- adjust size, duration, and time of day to ensure the canary gets enough requests to flag issues
- ensure the correct metrics are collected and monitored
    - should be able to indicate problems
    - should be attributable to the deployed changes, not external factors

## Identifying and Recovering from Overload
- symptoms of overload
    - unsustainable ticket backlog
    - poor team morale and health
- solutions
    - drop some tickets
    - drop support for noisy services (e.g. by returning it back to the dev team)
    - implement automation to reduce operational load
    - document common problems to allow self-service
    - improve team morale and culture
    
## SRE Engagement Model
- how SREs engage with a supported service
- SRE responsibilities in development phases:
    - architecture and design phase: enforce reliability best practices (e.g. no SPOF)
    - active development phase: **productionisation** (e.g. capacity planning, load balancing, monitoring, alerting, performance tuning)
    - limited availability phase: define SLOs, measure and evaluate metrics
    - general availability phase: operations
    - deprecation phase: support transition to new system, continue operations on old system
    - unsupported: delete references to the service in prod configs and documentation
- SRE <> Dev relationship requirements
    - goals and priorities
        - SRE short term goal: fulfill business needs in a reliable, available, scalable, and maintainable way
        - SRE long term goal: fully automate ops so they can move on to work on the next engagement
    - risks
    - ground rules (e.g. error budget, limits on operational work)
    - clear planning and execution (e.g. synced roadmaps)

## Reaching Beyond Your Walls
- communicate with customers using SLIs and SLOs
- build shared monitoring
- renegotiate SLOs after collecting and analysing metrics
- collaborate on design reviews
- practice disaster recovery

---
Source: https://www.goodreads.com/book/show/39687146-the-site-reliability-workbook
