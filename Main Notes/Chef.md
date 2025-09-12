Tags:
- [[Operating Systems]]
---
## What it is
- System management platform
- Represents infrastructure as code

## Fundamental Concepts
- Recipes: Codified instructions for how to set up a node
- Cookbooks: Package defining a a scenario and containing everything required to set up for that
    - recipes, metadata, attributes (specific details about a node, used to override default settings)
- Nodes: physical servers managed by Chef
- Roles: groups a set of recipes together, and applies them to every node with that role 

## Architecture
- Chef workstation: "interface" for devs / sysadmins to interact with Chef
- Chef Server: central hub
    - stores configs
    - receives and executes commands from the workstation
    - control plane the chef nodes
- Chef Node: machines managed by chef
    - each runs a Chef client
    - client **pulls** configs (e.g. recipes, roles, etc.) from Chef Server
---
## References
- https://www.techtarget.com/searchitoperations/definition/Chef-software
- https://amruthadronamraju.hashnode.dev/chef-a-configuration-management-tool-overview
- https://docs.chef.io/cookbooks/
- https://www.tutorialspoint.com/chef/chef_architecture.htm