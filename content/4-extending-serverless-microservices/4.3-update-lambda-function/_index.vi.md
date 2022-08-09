+++
title = "(Optional) Cập nhật quyền của hàm Lambda "
date = 2020
weight = 3
chapter = false
pre = "<b>4.3. </b>"
+++
#### (Optional) Cập nhật quyền của hàm Lambda 

1. Truy cập [**AWS Lambda Console**](https://console.aws.amazon.com/lambda) 
* Click **Functions**. 
* Nhập ```ImageManagerDemo``` vào ô tìm kiếm và nhấn **Enter**.
* Click tên của Lambda Function
![Deploy ImageManager Lambda Function](/images/4-extending-serverless-microservices/4.3-update-lambda-function/update-lambda-function-001.png?featherlight=false&width=90pc)
2. Chúng ta thấy rằng S3 trigger không xuất hiện mặc dù hàm vẫn chạy như mong muốn. Bởi vì hàm Lambda không biết về S3 bucket trigger - S3 biết về hàm Lambda mà nó sẽ kích hoạt khi một sự kiện diễn ra nhưng ngược lại thì không.
![Deploy ImageManager Lambda Function](/images/4-extending-serverless-microservices/4.3-update-lambda-function/update-lambda-function-002.png?featherlight=false&width=90pc)
3. Chạy lệnh sau để S3 trigger xuất hiện trong AWS Lambda
```
aws lambda add-permission --function-name <REPLACE_LAMBDA_FUNCTION_NAME> --region <REPLACE_REGION> --statement-id PolicyDocument --action "lambda:InvokeFunction" --principal s3.amazonaws.com --source-arn arn:aws:s3:::<REPLACE_S3_BUCKET_NAME> --source-account <REPLACE_AWS_ACCOUNT_ID> --profile
```
{{% notice note %}} 
Thay **<REPLACE_LAMBDA_FUNCTION_NAME>** bằng tên của **Lambda Function** có tên bắt đầu bằng **ImageManagerDemo-TestLambda** (tên của Lambda Function được tạo từ CloudFomation trong phần 4.2)
Thay **<REPLACE_S3_BUCKET_NAME>** bằng **idevelop-imagemanager-<YOUR_ACCOUNT_ID>** (tên của S3 bucket được tạo từ CloudFomation trong phần 4.2)
Thay **<REPLACE_AWS_ACCOUNT_ID>** bằng **AWS Account Id** của bạn
{{% /notice %}}
![Deploy ImageManager Lambda Function](/images/4-extending-serverless-microservices/4.3-update-lambda-function/update-lambda-function-003.png?featherlight=false&width=60pc)
4. Kiểm tra lại, chúng ta sẽ thấy S3 trigger xuất hiện.
![Deploy ImageManager Lambda Function](/images/4-extending-serverless-microservices/4.3-update-lambda-function/update-lambda-function-004.png?featherlight=false&width=90pc)