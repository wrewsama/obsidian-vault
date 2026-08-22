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

---
Source: https://www.goodreads.com/book/show/39687146-the-site-reliability-workbook
