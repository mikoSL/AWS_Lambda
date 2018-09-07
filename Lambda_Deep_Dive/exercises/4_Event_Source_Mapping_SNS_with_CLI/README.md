# Event Source mapping and SNS notification with CLI (lambda function creates snapshot for the instance and create a SNS topic)

## 1. connect EC2 with SSH
* copy the ip address of the running EC2 instance
```
$ ssh user_name@xx.x.x.x(ip address)
```
```
$ aws help
$ aws lambda help
```
## 2. Create lambda function
```
$ vim lambda_function.py
$ zip lambda_function.zip lambda_function.py
```
```
$ aws lambda create-function \
--region us-east-1 \
--function-name "EbsSnapshot" \
--description "create a snapshot when instance is shutting down" \
--zip-file fileb:///home/user_name/lambda_function.zip \
--role IAM_role_arn \
--handler lambda_function.lambda_handler \
--runtime "python3.6" \
--time 10 \
--memory-size 1024
```
## 3. Event source mapping
* create a rule
```
$ aws events put-rule \
--name "EC2-stopping" \
--event-pattern "{\"source\":[\"aws.ect\"], \"detail-type\":[\"EC2 Instance State-change Notification\"], \"detail\":{\"state\":[\"shutting-down\",\"stopping\"]}}" \
--state "ENABLED" \
--description "Take EBS snapshot of volume os instance shutting down" \
--region us-east-1
```

* add target (**add lambda function** to the rule above)
```
$ aws events put-targets \
--rule "EC2-stopping" \
--targets "Id"="1","Arn"="arn:aws:lambda:us-east-1:858444932944:function:EbsSnapshot" \
--region us-east-1
```
* add **permission** for the rule to invoke lambda
```
$ aws lambda add-permission \
--statement-id "nihaodf-123" \
--action "lambda:InvokeFunction" \
--principal events.amazonaws.com \
--source-arn lambda-function-arn \
--function-name EbsSnapshot \
--region us-east-1
```

* fill in the sns subscription

## 4. Shutdown instance
## 5. Event rule triggered
## 6. Invocation
## 7. Snapshot
## 8. Snapshot saved
