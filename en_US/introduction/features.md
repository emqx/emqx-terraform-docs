# 核心功能

- Infrastructure as Code
> When the resource state is maintained through code, the state version can be controlled and the infrastructure can be shared.

- Execution Plans
> Through the Plan check before the actual deployment, a list is generated that contains every component to be deployed, and unnecessary errors are avoided by checking this list.

- Resource Graph
> After building the graph of all resources, it can create and modify any resources that are not dependent on each other in parallel. Therefore, Terraform can efficiently build infrastructure, and operators can also deeply understand the dependencies in their infrastructure through the graph.

- Change Automation
> It can apply complex changesets to the infrastructure without manual interaction. Through the aforementioned execution plan and resource graph, we can know exactly what and in what order the Terraform will change, so as to avoid many possible human errors.