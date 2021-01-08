I didn't know you could do that with API gateway
by Eric Johnson

if you are just using API gateway to proxy a lambda, you are probably overpaying (use HTTP api - in preview)

types of APIs gateway
- Edge Optimized
- Private
  - only accessible from VPC
  - for internal use (microservices, etc)
- Regional
    - typical use cases
    - for building APIS for clients in the same region


on the back end, can connect to many different AWS services
or anything that has an HTTP endpoint

API gateways supports two protocals: HTTP and WebSocket

6 ways to create/configure API gateway
- AWS web console
- clould formation ***
- CLI
- Serverless Application Model ***
- CDK
- Swagger/openAPI

*** recommended

Integrating with backend services:
- lambda function
    - via proxy - gateway wraps request in metadata but does not change request
      - no handling of status codes, etc
    - via direct integration - requests can be modified by gateway, incoming to backend and outgoing from backend
      - modification is defined using VTL (something templating language) can change headers, body, whatever
      - for instance, can modify json formatting
      - use case: can validate http requests and reject if it fails
        - cheaper than using lambda to do validation and will reduce latency since there is one less hop (small improvement)
- HTTP
- Mock: respond to requests without a backend service

Features:
Authorization (can attach an "authorizer" to an API gateway
- open
- IAM permissions
- Cognito authorizer
- Lambda authorizers

Optimization:
you can attach a cache (just a setting on the gateway)

Throttling:
at account level per region, you max out of 10k requests per second w/ 5k burst (in same millisecond)
you can request a limit raise

you can override throttling at the method level (but cannot override account limit)
you can also throttle at client level, by looking at key

API gateway stages: don't use these unless you have to
always put production in a different account

Releasing:
you can do canary release (n% of traffic for X duration)

monitoring and metrics:
- cloudwatch
- X-Ray
- CloudTrail
- AWS Config

developer portal: creates documentation and can generate an SDK*** (in your chosen programming language) very interesting

you can configure custom domains
