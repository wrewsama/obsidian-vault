Tags:
- [[SRE]]
---
- catalog contains, for each service:
    - relevant people e.g. owners/SMEs/oncall duty personnel
    - dependencies
    - status (i.e. liveness check results, dashboards for metrics)
    - relevant links (e.g. runbooks, documentation)
- SRE owns the platform, but the devs in charge of each service own the data for their services
- strategy to get devs to keep their data updated: 
    - appeal to self-interest: include useful information/links like dashboards and runbooks so they will be likely to check the pages for their own services
    - reduce friction: prefill the data yourself on a best-effort basis (e.g. from git commit history) so devs only need to make minor corrections. Make the user experience for those corrections easy (auto validation, good documentation, etc.)
    - measure the adoption over time and adjust accordingly
---
Source: https://billduncan.org/backstage-1/ & https://billduncan.org/backstage-2/
