Tags:
- [[Distributed Systems]]
---
## What is it
- Workflow manager for data pipelines
- Represents workflows as DAGs where nodes represent tasks
- Workflows written as Python code

## Why is it useful
- Scheduling
- Dependency management
- monitoring
- **Scalability** over multiple workers

## Architecture
```
       ┌──────────┐    ┌───────┐    
       │ DAG code │    │Plugins│    
       │ (Python) │    │       │    
       └─▲────────┴◄─┐ └┬───┬──┘    
         │  sync     │  │   │       
         │           │  │   │install
         │           │  │   │       
       ┌─────────┐◄──┼──┘   │       
  ┌────┼Scheduler│   │      │       
  │    └─┬───────┘   │      │       
  │      │          ┌───────▼─┐     
  │      └─────────►│Workers  │     
  │    executes     └─────┬───┘     
  │                       │         
  │      ┌─────────────┐  │ io      
  └──────► metadata db │◄─┘         
   io    └────▲────────┘            
              │                     
              │                     
         ┌────┴───┐                 
         │Web App │                 
         └────────┘                 
```

#### Components
- Scheduler
    - schedules the tasks to be run by workers
    - take task dependencies into account
- Workers
    - execute tasks
- Metadata db
    - stores
        - DAG state
        - task instances
        - execution history
- Web app
    - display info
    - manage workflows manually

#### DAG code
- Python code
- synced to 
    - schedulers (need DAG info)
    - workers (need task info)
- schedulers and workers then sync with the metadata db, making the code visible on the webapp
- the sync is pull-based
---
## References
- https://www.cloudera.com/resources/faqs/apache-airflow.html
- https://airflow.apache.org/docs/apache-airflow/stable/core-concepts/overview.html