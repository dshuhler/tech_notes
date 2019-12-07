CON311

fargate is a "launch type" as opposed to the EC2 launch type, both within ECS

fargate removes need to manage - containers are first class citeizens
represents paradigm shift

containerization forces you to think about applications as completely decoupled
scaling at container level instead of VM / EC2 level

fargate: the underlying server is abstracted away
design paradigm shift: we need to decouple applications from the underlying host (meaning EC2 i guess)

1. decoupled networking: awsvpc networking mode
  w/ ECS you have 2 networking modes:
    - bridge networking mode - inside an EC2, there is a default network namespace. it has one physical network interface (called eth0)
      - any traffic incomring or outgoing to a container from inside or outside the EC2 instance has go through that shared eth0 interface
      - if you create 2 containers within the same ec2, they can only talk through the eth0 (host level network interface)
    - host mode
      - docker launches all containers in the same namespace. this allows containers in the same namespace to communicate without going through eth0
      - communiction


  solution: awsvpc networking mode
    - containers within ghte taks are in the same namespace can talk over local host
    - all traffic transits throught the task ENI - this does not rely on the host's eth0

    - can completely ignore host (ec2) components
    - now we have a task native primitive with no reliance on the host's networking stack

    - this means each task gets its own:
      - Elastic Network Interface (ENI)
      - IP address
      - Security Groups

2. decoulped IAM permissions: task role

    - before task roles, you would launch an ec2 instance with an instance IAM role
      - if you launch a task inside this EC2, the container would load the credentials of the ec2 instance role.
        - this means that if you have multiple tasks in the same ec2 that you want to have different access permissions, tough shit
          (instance role must have superset of all permissions needed by containers, which violates the principal of least privielge)
      - this is not compatible with fargate, since tasks shouldn't need to know what ec2 it is deployed in
   - now you can create an IAM role just for a specific task


3. decoupled monitoring: Task Metadata
-- datadog case study --
    - before task metadata: docker deamon runs within defautl network namespace. would have to install datadog daemon within that same network namespaces
    - when you launch the container, it is within it's own network namespace
    - datadog queries the docker daemon directly to get its statistics

    -> this doesn't work with fargate becasue datadog is running outside of the task sandbox
      - it is also quering outside of the task sandbox andpublishing through the default interface
      ** fargate requires the tasks to be totally decoupled from the host

    new integraton:
      - data dog agent is now installed in a container that can be pulled in as a "sidecar container" and actually runs within the task namespace
      - datadog agent still needs to communicate data to the outside world. so they added the "task metadata endpoint" that surfaces info about your task
      - datadog queries the task metadata endpoint to get info about the task, and communicates out through the task's network interface

Take away:
  - ask yourself: are you making assumptions about the underlying host?
    - you must be host agnostic to use fargate
