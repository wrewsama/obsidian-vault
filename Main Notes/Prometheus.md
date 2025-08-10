Tags:
- [[SRE]]
---
## What it is
- Monitoring + Alerting tool
- Pulls and stores metrics
- Offers query language (PromQL) to analyse those metrics

## Architecture

```
  ┌───────────────┐      ┌────────┐      
  │ svc discovery │      │targets │      
  └────────────▲──┘      └────▲───┘      
               │get targets   │pull      
            ┌──┼──────────────┴┐         
            │                  │         
            │    Prometheus    │         
            │                  │         
            │                  │         
            │  ┌───────────┐   │         
            │  │ TimeSeries│   │         
            │  │ DB        │   │         
            │  └───────────┘   │         
            └─┬────────────▲───┘         
              │            │             
              │alert       │ query       
              │            │             
    ┌─────────▼───┐      ┌─┼─────────┐   
    │ AlertManager│      │  Frontend │   
    └─────────────┘      └───────────┘   
```

- targets need to expose an endpoint that serves the metrics in the Prometheus exposition format
- can be done by instrumenting with Prometheus client libraries
---
## References
- https://youtu.be/STVMGrYIlfg?si=7pjTiiZ0YeQ0RUlh