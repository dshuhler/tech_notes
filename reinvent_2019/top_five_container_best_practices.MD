### Top Five Container Best Practices
talk by woman from new relic

starts off by her mentioning she is not a container expert and just googled for most of this

1. Build small container images
  - the smaller the image, the faster the time-to-build and time-to-pull
  - fewer security vulnerabilities
  - leverage the docker build cache. add your source code as late as possible so base image and dependencies can be cached
    - use multistage builds


2. leverage namepaces
  - docker creates default namespace. you should override this


3. set up health checks
  - "synthetics" check - go through load balancer
  - have separate "readyness" and "liveness"


4. leverage logs
  - provide more content for environmental behaviors

I don't know what the fifth one was. this was not a very good session
