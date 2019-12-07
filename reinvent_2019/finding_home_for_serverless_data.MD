### Finding a home for serverless data

Amazon Aurora serverless: why do I have to manage my database if I have a serverless app?
need to define a range of ACU - combination of memory, compute, storage
good for infrequent, intermittent, or unpredictable workloads
supports data API for SQL - you don't have to deal with connections or connection pooling


Dynamo:
check out 2018 session on dive into dynamo DB
  - "everyone is using dynamo db incorrectly"
  - you should almost always being using one table
  - initially built for amazon retail web shopping cart features - my session ID and what is in my cart

If you want to support SQL:
  - RDS
  - Aurora
  - Aurora Serverless
  - S3 (Athena, S3 select)
      - S3 select allows you to query a csv w/ query

If you need massive scale (100s or 1000s of reqs) or a flexible schema
  - DynamoDB
  - S3

Need to handle arbitrary objects
  - S3

Need event-driven data processing
  - Dynamo
  - S3

Don't want to manage servers
  - Aurora Serverless
  - DynamoDB
  - Why does RDS count as managing a server??

expecting infrequent, intermittent, or unpredictable workload OR I don't want to deal w/ database connections
  - Aurora Serverless
  - DynamoDB

I don't want to deal w/ database connections
   - Aurora Serverless
   - DynamoDB
   - no connection pooling w/ serverless

Serverless best practices w/ RDS:

- initialization code before lambda_handler() is executed once per execution environment provisioning  
- second call to a warm lambda will not execute initialization code
creating the connection is expensive so we don't want to create it over and over
- need to make sure you close the connection
- do lazy instantiation - maybe the lambda does not always require a connection to the database (like db read only happens IF)
- consider using a "singleton" connection - wrap your connection in a class so we don't accidentally create 2 connections w/ the same lambda

externalizing database credentials:
  - can pull these out of the environment variables
  - use ssm parameter store
  - secrets manager for username and password

Make sure you commit/rollback you transactions because the next invocations might use that connection and commit/rollback your transaction

Aurora:
you will have 2 endpoints, one for writing and one for reading
 - this makes logic more complicated
 - can do "proxy sql" - google "proxy sql amazon RDS"

Aurora serverless:  
** use the data API endpoint
- in his opinion, this is the future since you don't want to open connections to database w/ serverless
- don't need to worry about how many connections are open, managing connections, etc
- you do have to pass in the credentials every time, so wrap the calls in a custom method and put it in a library to make code clean
- since you are now doing HTTP calls instead of TCP, there is more overhead per query, so batching becomes more important
- you can still do transactions, but you need to programmatically start a transaction and get a transaction ID and supply that in subsequent calls

DynamoDB:
- use IAM instead of credentials - every lambda has a IAM role and this is where permissions should live
- dynamo is eventually consistent
  - case: add employee to table then immediately try to get them from database - can fail since not all replicas might have it.
    - if you configure dynamo to be strongly consistent (must wait for all replicas to have element) it hurts performance. cost will also go up
- can use aws xray to trace DB calls
- you can run dynamo locally using docker (will have to connect w/ a URL)

S3:
can create s3 events that will trigger a lambda using AWS SAM (serverless application model)
- cloudwatch looking yml defines what happens
