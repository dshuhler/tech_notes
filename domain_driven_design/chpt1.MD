This is about working with domain experts to develop a model

- You have to learn their language and domain, but they have to learn a bit about yours to make communication better. (example: explaining ERD notation and class diagrams)
- tactic: focus on one feature for a while
- trial and error is necessary. There will be a lot of back and forth. walk thru scenarios.
- write a simple prototype to demo to domain experts. do not worry about infrastructure or persistance or UI

Keys:
1. Bid the model and implementation. This avoids the need for mental translation between one and the other
1. Cultivate a language based on the model.
1. Develop knowlege-rich model. The objects should have behavior and rules. It should not be just a data schema.
1. Distill the model. drop concepts that are not needed
1. Brainstorm and Experiment
  1. go thru scenarios using the language of the model. Watch for awkward expressions that are red flags.

Places info comes from when designing a new system:
1. Domain experts
1. Users of existing systems
1. experience of technical team of legacy or related systems
1. documentation
1. lots and lots of talk

waterfall method: SMEs talk to analysts, analysts do the modeling and then give it to programmers. it fails because it lacks feedback.

programmers should not just build features the SMEs want. They must build useful domain abstractions if they want to add powerful new features.

A rich domain model makes it easier for developers to learn about the domain. Projects leak knowlege as people leave and documentation gets stale. code is always the truth.

Highly productive teams practice continuous learning.

Knowledge-Rich Design
- must go beyond "find the nouns"
- business activities and rules are as important as data
- it is important to use abstractions to make things understandable



there is a good example of pulling in-line logic out into a domain object that makes things easier to read
the model can change profoundly as development progresses and this should not be looked at as a failure, but a good thing.

Points for discussion group:
- this is really just an intro to DDD to get a feel for it. this is a classic and is associated with the move to microservices: bounded context as service boundary
- he starts by building prototype with no infrastructure, GUI, or persistence
  - I did this for publishing rights and Erin immeadiately pointed out error in representation - despite us discussing topic at length
    - this also goes to the point that the model is more than an ERD and data dictionary

- Don't be afraid of throwing bad models away. "Why didn't you get it right the first time?" is ignorant criticism. it's impossible to get it right the first time.
