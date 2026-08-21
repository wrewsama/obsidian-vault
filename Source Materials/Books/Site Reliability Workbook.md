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
    
---
Source: https://www.goodreads.com/book/show/39687146-the-site-reliability-workbook
