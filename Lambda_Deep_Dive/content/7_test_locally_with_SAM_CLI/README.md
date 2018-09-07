# Lambda test locally (SAM CLI)

## start-api
* launch a **Docker containter** locally.
* allow reinvoking without redeployment.
```
$ sam local start-api
```
```
$ curl http://localhost:3000/products
```
```
$ curl -XPOST http://localhost:3000/products/2018
```
## invoke
```
$ sam local invoke "Function"
```
## generate-event
```
$ sam local generate-event api > event.json

# "products" is the name of the Function
$ sam local invoke Products -e event.json

```
# validate
# package and deploy **different in the following syntax**
```
$ sam package \
--template-file template.yaml \
--output-template-file serverless-output.yaml \
--s3-bucket bucket-name
```
```
# with CloudFormation 

$ aws sam deploy \
--template-file serverless-output.yaml \
-- stack-name xxxx \
--capabilities CAPABILITY_IAM
``
