# Invoke Lambda

* **Synchronous** invocation type: orders matter! A return is expected and needed. (e.g. API Gateway)
* **Asynchronous** invocation type: only need to know function is invoked. order is not important. (e.g. upload object to S3 bucket)
* **Predetermined** invocation type-- can not be changed!
* Event model: **push**(e.g.CloudWatch rule trigger Lambda) and **pull** (e.g. lambda pull from kinesis to pass failed events to DLQ)
* **Event source mapping**: setting up event sources and configuring which function is triggered.
