#### Serverless Architecture Best Pracices
by lead aws serverless guy

these patterns are used by 80% of serverless users

Jeremy Daly has more in depth patterns

Pattern: the comfortable REST
- use "Lambda Power Tuning" to determine what memory you need

use AWS app sync to cache graphQL requests

when you are talking to a non-scalable resources, limit concurrency
use Kenisis data streams as an event source
use Kinesis as a buffer and a better mechanism to limit concurrency
use lambda destination instead of dead letter queue

check out realworld-serverless-application (should be in java)

very tough to follow this guy, not a very good session
