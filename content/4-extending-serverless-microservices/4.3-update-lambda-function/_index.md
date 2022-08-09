+++
title = "(Optional) - Update Lambda function permissions"
date = 2020
weight = 3
chapter = false
pre = "<b>4.3. </b>"
+++
#### (Optional) - Update Lambda function permissions


1. Go to [**AWS Lambda Console**](https://console.aws.amazon.com/lambda) 
* Click **Functions**. 
* Type ```ImageManagerDemo``` to the search bar and press **Enter**.
* Click the name of the Lambda Function
![Deploy ImageManager Lambda Function](/images/4-extending-serverless-microservices/4.3-update-lambda-function/update-lambda-function-001.png?featherlight=false&width=90pc)
2. If we view the triggers against the Lambda function in the AWS console, we will notice that the S3 trigger does not appear, even though it functions as expected. This is because the Lambda function itself does not know about the S3 bucket trigger - S3 knows about the Lambda function it will trigger when an event fires, but not the other way around.
![Deploy ImageManager Lambda Function](/images/4-extending-serverless-microservices/4.3-update-lambda-function/update-lambda-function-002.png?featherlight=false&width=90pc)
3. Execute the following command
```
aws lambda add-permission --function-name <REPLACE_LAMBDA_FUNCTION_NAME> --region <REPLACE_REGION> --statement-id PolicyDocument --action "lambda:InvokeFunction" --principal s3.amazonaws.com --source-arn arn:aws:s3:::<REPLACE_S3_BUCKET_NAME> --source-account <REPLACE_AWS_ACCOUNT_ID> --profile
```
{{% notice note %}} 
Repalce **<REPLACE_LAMBDA_FUNCTION_NAME>** with the name of the **Lambda Function** whose name starts with **ImageManagerDemo-TestLambda** ( the Lambda Function was created from CloudFomation in the section 4.2)
Repalce **<REPLACE_S3_BUCKET_NAME>** with **idevelop-imagemanager-<YOUR_ACCOUNT_ID>** (The name of the S3 bucket was created from CloudFomation in the section 4.2)
Repalce **<REPLACE_AWS_ACCOUNT_ID>** with your **AWS Account Id**
{{% /notice %}}
![Deploy ImageManager Lambda Function](/images/4-extending-serverless-microservices/4.3-update-lambda-function/update-lambda-function-003.png?featherlight=false&width=60pc)
4. if you recheck, you will see the S3 trigger listed.
![Deploy ImageManager Lambda Function](/images/4-extending-serverless-microservices/4.3-update-lambda-function/update-lambda-function-004.png?featherlight=false&width=90pc)