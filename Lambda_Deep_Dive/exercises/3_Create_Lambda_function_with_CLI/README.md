# Create lambba function with CLI, developer create function locally then upload it to lambda with CLI.

## 1. connect EC2 with SSH
* copy the ip address of the running EC2 instance
```
$ ssh user_name@xx.x.x.x(ip address)
```
```
$ aws help
$ aws lambda help
```
```
$ aws lambda list-functions \
--region us-east-1
```

## 2. Create lambda function
```
$ vim lambda_function.js
$ zip lambda_function.zip lambda_function.js
```
```
$ aws lambda create-function \
--region us-east-1 \
--function-name "listS3Buckets" \
--runtime "nodejs8.10" \
--role "role-arn xxxxxxxxx" \
--handler "lambda_function.handler" \
--zip-file fileb:///home/user_name/lambda_function.zip
```
```
$ aws lambda update-function-configuration \
--region us-east-1 \
--function-name "listS3Buckets" \
--description "create our s3 function with CLI" \
--timeout 5 \
--memory-size 256
```

## 3. Invoke function
```
$ aws lambda invoke \
--region us-east-1 \
--function-name "listS3Buckets" \
OUTFILE.log
```
## 4. Function gets all bucket name within region
## 5. Logged to CloudWatch
## 6. Function return result
