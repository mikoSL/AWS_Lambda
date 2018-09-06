# Lambda CLI

## 1.create function
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
## 2. update function (need permission **UpdateFunctionCode**)
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
## 3. view lambda CloudWatch Logs using AWS CLI
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
## 4. pull & push events and permission
### pull (for streaming service, e.g. kinesis or DynamoDB)
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
### push
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
## 5. invoke function
```
$ aws lambda invoke \
--function-name xxx
--invocation-type Event \
--payload xxx.json \
OUTFILE.log
```
