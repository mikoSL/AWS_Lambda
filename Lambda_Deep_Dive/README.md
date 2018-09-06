# AWS Deep Dive course from linuxacademy.com

[Content](https://github.com/mikoSL/AWS_Lambda/tree/master/Lambda_Deep_Dive/content)
* [Lambda Introduction](https://github.com/mikoSL/AWS_Lambda/tree/master/Lambda_Deep_Dive/content/1_introduction)
* [Lambda overview and creation](https://github.com/mikoSL/AWS_Lambda/tree/master/Lambda_Deep_Dive/content/2_overview_creation)
* [Logging and monitoring with Lambda](https://github.com/mikoSL/AWS_Lambda/tree/master/Lambda_Deep_Dive/content/3_logging_monitoring)
* [Invoke Lambda](https://github.com/mikoSL/AWS_Lambda/tree/master/Lambda_Deep_Dive/content/4_invoke_Lambda)
* [CloudWatch Event & Lambda](https://github.com/mikoSL/AWS_Lambda/tree/master/Lambda_Deep_Dive/content/5_cloudwatch_event_lambda)
* [Lambda CLI](https://github.com/mikoSL/AWS_Lambda/tree/master/Lambda_Deep_Dive/content/6_lamba_CLI)

[Exercises](https://github.com/mikoSL/AWS_Lambda/tree/master/Lambda_Deep_Dive/exercises)
* [EC2_snapshot](https://github.com/mikoSL/AWS_Lambda/tree/master/Lambda_Deep_Dive/exercises/EC2_snapshot)


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
* monitoring with CloudWatch
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

## CloudWatch Event & Lambda
* Event pattern
* Scheduled expression (Cron expression & rate expression)
* Target
* Configure CloudWatch alarm
* Rule can be set in CloudWatch event or in Lambda

## Lambda CLI

### 1.create function
```
$ aws lambda create-function \
--function-name "xxxxx"
--runtime "python 3.6"
--role "copy role ARN here"
--handler "xxx.xxxx (file_name.handler_name)"
--code "s3bucket= xxxxx, s3key=xxxx(file name)"
```
```
$ aws lambda list-functions
```
```
$ aws lambda get-function --function-name "xxx"
```
```
$ aws lambda get-function-configuration --function-name "xxx"
```
### 2. update function (need permission **UpdateFunctionCode**)
```
$ aws lambda update-function-code \
-- function-name "xxxx"\
--zip-file fileb:///User/xx/xxx.zip
```
```
$ aws lambda update-function-configuration \
--function-name "xxxx"\
--description "updated"\
--timeout "10sec"
```
* **create-event-source-mapping** only for **STREAMING** service.
```
$ aws lambda create-event-source-mapping \
--event-source-arn "arn:xxxx" \
--function-name "xxxx"
```
```
$ aws lambda update-event-source-mapping \
--uuid "arn:xxxx"
--no-enabled \
--function-name "xxxx"
```
### 3. view lambda CloudWatch Logs using AWS CLI
```
$ aws logs describe-log-groups | less
```
```
$ aws logs describe-log-stream \
--log-group-name xxxx
```
```
$ aws logs describe-log-stream \
--log-group-name xxxx
```
* **Tips** with alias and if
```
$ log_streams=$(aws logs describe-log-streams \
  --log-group-name xxxx \
  --output text \
  --query 'logStreams[*].logStreamName') && log_stream in $log_streams; \
  do aws logs get-log-events \
  --log-group-name xxxxx
  --log-stream-name $log_stream --output text \
  --query 'events[*].message'; \
  done | less
```
### 4. pull & push events and permission
#### pull (for streaming service, e.g. kinesis or DynamoDB)
```
$ aws lambda create-event-source-mapping \
--function-name "xxxx"
--event-source-arn "xxxxx"
--starting-position LATEST
```
```
# only show streaming based service
$ aws lambda list-event-source-mapping \
--function-name "xxxx"
```
#### push
* 1. create lambda function
* 2. create permission using **aws s3api** CLI not **aws s3** CLI
```
$ aws s3api put-bucket-notification-configruation \
--bucket "xxxx"
--notification-configuration file:///User/xx/xx.json
```
```
$ aws s3api get-bucket-notification-configruation \
--bucket "xxxx"
```
* 3.create event source
```
#upload file to s3 bucket
$ aws s3 cp /User/.../xx.jpg \
s3://bucket-name
```
### 5. invoke function
```
$ aws lambda invoke \
--function-name xxx
--invocation-type Event \
--payload xxx.json \
OUTFILE.log
```
