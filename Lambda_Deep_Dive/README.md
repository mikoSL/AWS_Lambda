# AWS Deep Dive course from linuxacademy.com

## Lambda Introduction
### Limit
 1. Resource limit per invocation
 2. Account limit per region
 3. Deployment Limit
### Retries
 1. **Synchronous** failure (429 errors)
 2. **Asynchronous** failure: events queued before being used to invoke function, retried twice with delays, then send to **DLQ(Dead Letter Queue)** if configured.
 3. **Stream-based** failure: retry until data expires(diff per service), **block** and **not read** new records until retry succeeds. Order is ensured using the blocking principle.

## Lambda overview and Creation
* Function must be **stateless** (meaning no data is saved in the function) when being created!
* Be aware of **cold start** time, 1st invocation are the slowest.
* Fundamental components:
 1. desinger(graphically setup triggers)
 2. function code (**handler**-- name_of_file.name_of_function)
 3. environment variables
 4. tags (**group** and **filtering**! Important!)
 5. execution role
 6. basic settings
 7. network
 8. DLQ (Dead Letter Queue)
 9. auditing/compliance (cloudTrail)
 10. concurrency **(calculating concurrency for non-stream**: average execution time * number of events processed. e.g. 100 events from API Gateway, 3 sec/event, then it is 100*3 = 300) (**concurrency for stream based service**:100 records being read per stream from DynamoDB, 50 active shares at a given time-- **50 concurrent executions** Each active shard is considered a concurrent execution.) (**Throttling** concurrency)

## Logging and monitoring with Lambda
* monitoring with cloudWatch
* logging in the test execution result.
* CloudWatch console
* X-Ray overview (downstream to S3 bucket)
 1. 3 variables used by AWS for interacting with X-Ray
 2. Need to include **X-Ray SDK in your code**.
 3. An X-Ray daemon run automatically in your Lambda environment, collecting data.
 4. The daemon consumes either **16MB** or **3% of allocated memory**.

## Invoke Lambda
* **Synchronous** invocation type: orders matter! A return is expected and needed. (e.g. API Gateway)
* **Asynchronous** invocation type: only need to know function is invoked. order is not important. (e.g. upload object to S3 bucket)
* **Predetermined** invocation type-- can not be changed!
* Event model: **push**(e.g.CloudWatch rule trigger Lambda) and **pull** (e.g. lambda pull from kinesis to pass failed events to DLQ)
* **Event source mapping**: setting up event sources and configuring which function is triggered.
