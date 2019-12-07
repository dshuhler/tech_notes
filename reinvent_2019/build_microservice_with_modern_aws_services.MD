ENT308-S - Build your next microservices application with modern AWS services

when you are building a service, start with a "why?"
  - do you need super scalability?

Definition of a modern microservice:
  1. devops: leverages a code pipeline
  1. autoscaled: doesn't require manual scaling (example: EC2 autoscaling groups)
  1. cloud-native: based on cloud-compute (example: Lambda and CloudFront)
  1. orchestrated: low cost of operational burden (example: EKS)

their example service uses EKS + Lambda + Fargate
Benefits:
  - Operations: Don't have to worry about underlying servers
  - Cost: No traffic, No cost
  - Productivity: higher abstraction

add "bedrock" of observability  
monitoring: goal is "single pane of glass" that has everything you need. not really achievable but a guideline  
(they are selling New Relic here)

New relic can plug into all these managed AWS services including sqs queues, lambdas, and containers

*** this integration looks really nice - is anyone at SX using this?
