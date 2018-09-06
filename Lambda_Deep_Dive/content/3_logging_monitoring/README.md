# Logging and monitoring with Lambda
* monitoring with CloudWatch
* logging in the test execution result.
* CloudWatch console
* X-Ray overview (downstream to S3 bucket)
 1. 3 variables used by AWS for interacting with X-Ray
 2. Need to include **X-Ray SDK in your code**.
 3. An X-Ray daemon run automatically in your Lambda environment, collecting data.
 4. The daemon consumes either **16MB** or **3% of allocated memory**.
