### Improving Observability Of Your Containers (CON328)

What do we need to be able to observe?
  - application level: each service and between microservices
  - container service level: services, tasks, pods
  - compute service level: cluster, instances

##### AWS App Mesh
**service mesh** to manage communication between microservices. basically, it acts as
a proxy that all service communication passes through - for instance, between load balancer
and service instance / between service instance and another service instance.
 - service mesh can collect metrics, logs, and tracing at **application level**
 - AWS App Mesh works with Cloudwatch (via Cloudwatch agent)
 - AWS App Mesh works with 3rd party tools like datadog (via Statsd agent)

example dashboards show information at service level and also inter-service level

##### FireLens and AWS for Fluent built
problems:
1. need support for wider array of analytics for ECS/fargate
2. need to decouple configuration and lifecycle management of telemetry software from underlying compute

current approaches for dealing with logging in containers:
1. build log drivers into the container image
2. write log to certain place on disk and use a sidecar to read the logs off that location

these approaches were not scalable - need to decouple from applications

**FireLens**
- send logs natively to AWS and APN partner log analytics and storage tools
- can push logs to cold storage and pull on demand (saves money)
- decoupling: can configure log routing separately through a config file that is independent of the application

##### AWS Container Insights
fully managed observability service
use cases:
 - infrastructure level
  - cluster CPU utilization / memory consumption
 - container service level
  - service task count
  - task memory utilization
 - application level
  - application logs
