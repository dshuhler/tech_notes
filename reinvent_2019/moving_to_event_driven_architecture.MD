### Moving to an Event Driven Architecture

example: amazon retail site - people click buy buttons faster then the warehouses can output packages  
amazon needs:
- scalable ingestion
- reliable storage
- fan-out

don't need:
- strong ordering (ok if orders are filled in different order than they are placed)
- deduplication
- push delivery (warehouse wants to pull events)
- ultralow latency

Is event driven for you?
strong indicators:
1. are you passing around self-contained transactions (not related to events before or after)
  - so you can code things in a stateless way
  - event driven tends to make things more stateless
2. Are useful events available for free?
  - go with the flow in this case
3. How strong do you need to decouple your microservices?
  - eventing is probably the best way to get clean decoupling
4. Do you need publish/subscribe?

What is an "event"?
- a state change
- something a user did  

very difficult to characterize. the software doesn't know what it is.

-> check out cloudevent for a concrete example

once you start shipping events to different systems, it is nearly impossible to change the schema of the event

event processing facets
1. strict ordering, basically FIFO or not
  - strict FIFO only really works with one sender and one receiver (will be more expensive and less scalable)
2. deduplication
  - sometimes you will get a message twice because of a network partition, etc
  - best way to deal with it is make API idempotent
  - even with FIFO, you can get duplicates due to service failures
3. Point-to-point or publish/subscribe
  - with point to point, only one receiver will see it
4. push vs pull
  - sqs you pull from
  - sns pushes
  - push sounds desirable but cloud engineers get worried about is since now you are promising to be available and ready to go at all times
  - polling allows you to only take messages when you have the capacity to process
5. latency profile
  - average latency is a bad measure (you will often see big outliers which you need to be able to handle)
  - you should like at the percentiles over a time window
6. serverless vs broker/cluster
  - rabitmq, activemq
  - disadvantages of brokers: you have to have a running machine all the time, you have to manage it, it has a limit to how much it can scale (it will top out)
  - however, a decent EC2 can handle a huge amount of messages with a broker so maybe you don't care
  - brokers can have lower latency because they use TCP (Http long polling and HTTP2 and QUIC (HTTP3) may mitigate this)
7. Filtering vs firehouse
  - case: consumer ends up discarding 90% of messages because they don't care. you could move the filtering to a subscription
8. Data blogs or structured objects
  - messages are typically json
  - but binary formats are popular (like AVRO)
  - widely held belief that binary formats are faster but the support for this is pretty weak and depends on the type of data. If you really
  care about this is that you should do an experiment with you data. you will probably find no difference and this is typically not a bottleneck anyway.
  so I guess just use json.
9. records or documents
 - records (table structure that reflects thinking in SQL)
 - documents (has deeply nested and repeating structures) - this tends to be a "culturally" better fit for events
10. uniform vs heterogenous events
 - do you have different kinds of events on the same event bus?
 - majority of event busses have different kinds of events, this means developer has to figure out what kind of event it is which kind of sucks
    - not a good use of dev time. this is a source of discomfort
    - you can use AWS schema registry to give events a strong type
      - cheap enough that is it functionally free but is still in preview
      - can generate code to deserialize message into (java, node, python currently supported)


he frames this as APIs vs. Events

defining API in this context: it is synchronous - we are frozen until API returns

pattern: if you want a response to a message, put a field in the message that contains the URL of the queue that you want the response to come back on

how to deal with brittleness of APIS: use service mesh. rather than calling each other based on dns names or IP address, you just call a service name and add event triggers and other functionalities

things that still need work (hard to do with events):
1. loops (infinite loops where messages get re-added to queues, can cause big and really expensive problems)
2. too much pipefitting: lots of dumb lambdas that don't do much and add complexity
3. easier and safer access control: hard to secure lots of separate AWS resources
4. blobbly events, discovery and autocomplete needed
