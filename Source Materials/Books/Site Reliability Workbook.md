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
---
Source: https://www.goodreads.com/book/show/39687146-the-site-reliability-workbook
