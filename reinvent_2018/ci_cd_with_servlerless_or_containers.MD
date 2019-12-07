term "modern application" - what does it mean?

- Simplify environment management with serverless technologies
  - two serverless options in AWS: Lambda and Fargate
- Reduce impact of code changes with microservices architecture
- Automate operations by modeling applications and infrastructure as code
- accelerate the delivery of new, high-quality services with CI/CD
- Gain insight across resources and applications by enabling observability
- protect customers and the business with end-to-end security and compliance

Continuous integration:
  1. SOURCE phase
    - check in source code
    - peer review source code
  2. BUILD phase
    - compile code
    - unit test
    - style check
    - create container images and function deployment packages
  - still have manual test and deployment steps

Continuous Delivery (she says "deployment"):  
her definition requires that the last time a code change is touched is when it is checked into repo  
adds:
  3. TEST phase
    - integration tests with other systems
    - load testing
    - UI tests
    - security testing
  4. PRODUCTION
    - deployment to production environment
    - monitor code in prod environment

effects of CI/CD according to 2018 state of devops report
  1. deployment frequency goes from weekly/monthly to hourly/daily
  2. lead time goes from 1-6 months to 1-7 days
  3. change failure rate goes from 40$ - 60% to 0% - 15%

Pillars of releasing modern applications:
1. Continuous Integration
  1. automatically kick of a new release when new code is checked in
  2. build and test code in a consistent, repeatable environment
  3. continually have an artifact ready for deployment
  4. continually close feedback loop when build fails
2. Continuous Deployment
  1. Automatically Deploy new changes to staging environments for testing
  2. Deploy to production safely without impacting customers
  3. Deliver to customers faster: increase deployment frequency, and reduce change lead time and failure rate

You have to be very careful with "prod" or "latest" tag. scenario:
  1. prod is happily running 3 tasks with current "latest" tag
  2. a build passes all gates and gets tagged with "latest"
  3. prod has to scale up. it creates new tasks with whatever is tagged "latest" - meaning the new tasks will be using the untested new "latest" which will be different from the "latest" images already running in the service - bad!

*** to avoid: use immutable tags like a build ID, or just use the image ID (which is just a SHA256 digest of image)

good pattern:
  1. push new image with new build ID (prod is running with older build ID)
  2. when we do deployment, we update the service's task definition, replacing tasks

Infrastructure as Code:
goals:
  - make infra changes repeatable and predictable
  - release infra changes using the same tools as code changes
  - replicate production environment in a staging environment to enable continuous testing

what does continuous testing mean in the context of infra as code?
- Validate an artifact (build stage)
  - unit tests
  - static analysis
  - mocked dependencies and environments
  - vulnerability image scans

- Validate an environment  (test stage)
  - integration steps against real dependencies and real environment
  - load testing
  - penetration testing
  - monitoring to test impact of deployments on environments
