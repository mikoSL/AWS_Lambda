# Lambda overview and Creation

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
