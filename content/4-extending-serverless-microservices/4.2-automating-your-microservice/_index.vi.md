+++
title = "Tự động hóa Serverless Microservice"
date = 2020
weight = 2
chapter = false
pre = "<b>4.2. </b>"
+++
#### Tự động hóa Serverless Microservice

Trong bài tập trước, bạn đã sử dụng IDE Eclipse để tạo một hàm Lambda bằng Bộ công cụ AWS cho Eclipse và cập nhật một hàm Lambda. Điều này cho phép bạn khởi tạo việc tự động tải lên hàm Lambda của mình theo cách thủ công. Tuy nhiên, cơ chế này có thể không thuận tiện cho việc tự động hóa các bước triển khai cho các hàm hoặc phối hợp triển khai và cập nhật cho các phần tử khác của ứng dụng serverless, chẳng hạn như event sources và downstream resources. Ví dụ: IDE Eclipse không cung cấp cho bạn khả năng triển khai và cập nhật S3 bucket và kết nối S3 PUT OBJECT trigger, cùng với hàm Lambda của bạn như một đơn vị triển khai.

Bạn có thể sử dụng **AWS CloudFormation** để dễ dàng chỉ định, triển khai và định cấu hình các ứng dụng serverless. AWS CloudFormation là một dịch vụ giúp bạn lập mô hình và thiết lập các tài nguyên Amazon Web Services để bạn có thể dành ít thời gian hơn cho việc quản lý các tài nguyên đó và nhiều thời gian hơn để tập trung vào các ứng dụng chạy trong AWS của bạn. Bạn tạo một mẫu ( **Template** ) mô tả tất cả các tài nguyên AWS mà bạn muốn (như các hàm Lambda và nhóm S3) và AWS CloudFormation sẽ đảm nhận việc cung cấp và cấu hình các tài nguyên đó cho bạn.

Ngoài ra, bạn có thể sử dụng **AWS Serverless Application Model (SAM)** để thể hiện các tài nguyên bao gồm ứng dụng serverless. Các loại tài nguyên này, chẳng hạn như các hàm và API của AWS Lambda, được AWS CloudFormation hỗ trợ đầy đủ và giúp bạn xác định và triển khai ứng dụng serverless của mình dễ dàng hơn.

Trong phần này, bạn sẽ sử dụng **AWS CLI** và **AWS CloudFormation/SAM** để đóng gói ứng dụng và triển khai nó từ đầu mà không cần phải tạo hoặc định cấu hình bất kỳ phần phụ thuộc nào theo cách thủ công.

1. Trong Eclipse IDE, click chuột phải vào project **TestLambda**
* Click **New**
* Click **File**
![Deploy ImageManager Lambda Function](/images/4-extending-serverless-microservices/4.2-automating-your-microservice/automating-your-microservice-001.png?featherlight=false&width=90pc)
2. Tại mục **File name**, điền ```template.yaml```
* Click **Finish**
![Deploy ImageManager Lambda Function](/images/4-extending-serverless-microservices/4.2-automating-your-microservice/automating-your-microservice-002.png?featherlight=false&width=90pc)
3. File **template.yaml** sẽ nằm cùng cấp với file **pom.xml** của project **TestLambda**
* Thay nội dung file **template.yaml** bằng nội dung dưới đây
```
AWSTemplateFormatVersion: '2010-09-09'
Transform: 'AWS::Serverless-2016-10-31'
Description: Testing lambda and S3
Resources:
  TestLambda:
    Type: 'AWS::Serverless::Function'
    Properties:
      Handler: idevelop.lambda.s3handler.LambdaFunctionHandler
      Runtime: java8
      CodeUri: target/s3handler-1.0.0.jar
      Description: Testing lambda and S3
      MemorySize: 512
      Timeout: 15
      Role: !Sub arn:aws:iam::${AWS::AccountId}:role/LambdaRole
      Events:
        CreateThumbnailEvent:
          Type: S3
          Properties:
            Bucket:
              Ref: ImageManagerSrcBucket
            Events:
              - 's3:ObjectCreated:Put'
            Filter:
              S3Key:
                Rules:
                  - Name: prefix
                    Value: uploads/

  ImageManagerSrcBucket:
    Type: AWS::S3::Bucket
    Properties:
      BucketName: !Sub idevelop-imagemanager-${AWS::AccountId}
```
* Lưu lại
![Deploy ImageManager Lambda Function](/images/4-extending-serverless-microservices/4.2-automating-your-microservice/automating-your-microservice-003.png?featherlight=false&width=90pc)
4. Chúng ta sẽ sử dụng AWS CLI để đẩy tập tin **template.yaml** lên S3 bucket nơi mà nó có thể được triển khai. 
* Trong Command prompt, chạy câu lệnh dưới đây
```
aws cloudformation package --template-file template.yaml --s3-bucket <YOUR_CODE_BUCKET_NAME> --output-template deploy-template.yaml --profile devaxacademy
```
{{% notice note %}} 
Thay **<YOUR_CODE_BUCKET_NAME>** bằng tên **S3 bucket** có tên bắt đầu bằng **idevelop-sourcecode** chúng ta đã tạo
{{% /notice %}}
![Deploy ImageManager Lambda Function](/images/4-extending-serverless-microservices/4.2-automating-your-microservice/automating-your-microservice-004.png?featherlight=false&width=60pc)
{{% notice note %}} 
Nếu bạn thấy một thông báo lỗi **‘NoneType’ object has no attribute ‘items’** hãy kiểm tra lại format của tập tin **template.yaml**
{{% /notice %}}

{{% notice info %}} 
Câu lệnh **aws cloudformation package** sẽ lấy mẫu AWS SAM được cung cấp và viết lại nó trong định nghĩa của artifact được tự động tải lên S3 bucket. Trong trường hợp này, **deploy-template.yaml** được tạo và chứa giá trị **CodeUri** trỏ đến tập tin zip triển khai trong Amazon S3. Mẫu này đại diện cho ứng dụng serverless của bạn
{{% /notice %}}
5. Bây giờ bạn đã sẵn sàng triển khai tập tin JAR dưới dạng một hàm Lambda và kết nối S3 trigger vào một S3 bucket mới. Bạn sẽ nhận thấy kết quả từ lệnh trước đó hướng dẫn chúng ta những gì cần chạy để triển khai mẫu đóng gói.
* Trong Command prompt, chạy câu lệnh dưới đây
```
aws cloudformation deploy --template-file deploy-template.yaml --stack-name ImageManagerDemo --profile devaxacademy
```
![Deploy ImageManager Lambda Function](/images/4-extending-serverless-microservices/4.2-automating-your-microservice/automating-your-microservice-005.png?featherlight=false&width=60pc)
6. Truy cập [**Amazon CloudFormation Console**](https://console.aws.amazon.com/cloudformation/home).
* Click **Stacks**
* Nhập ```ImageManagerDemo``` vào thanh tìm kiếm và nhấn **Enter**
* Chúng ta sẽ thấy CloudFormation stack có tên **ImageManagerDemo** được tạo
![Deploy ImageManager Lambda Function](/images/4-extending-serverless-microservices/4.2-automating-your-microservice/automating-your-microservice-006.png?featherlight=false&width=90pc)
{{% notice info %}} 
Bạn có thể xem quá trình tạo tài nguyên trực tiếp trong bảng điều khiển CloudFormation. \
CloudFormation template tạo một S3 Bucket có tên **idevelop-imagemanager-<YOUR_ACCOUNT_ID>** trong đó **<YOUR_ACCOUNT_ID>** là **AWS account ID** của bạn.\
Template cũng tạo một hàm Lambda mới có tên là **ImageManagerDemo-TestLambda-XXXXXX** trong đó **XXXXXX** là một mã định danh ngẫu nhiên được CloudFormation phân bổ để đảm bảo tính duy nhất của tên hàm.
{{% /notice %}}
7. Truy cập [**AWS S3 Console**](https://s3.console.aws.amazon.com/s3/)
* Nhập ```idevelop-imagemanager``` vào ô tìm kiếm
* Click S3 bucket **idevelop-imagemanager-<YOUR_ACCOUNT_ID>**
![Deploy ImageManager Lambda Function](/images/4-extending-serverless-microservices/4.2-automating-your-microservice/automating-your-microservice-007.png?featherlight=false&width=90pc)
8. Click **Create folder**
![Deploy ImageManager Lambda Function](/images/4-extending-serverless-microservices/4.2-automating-your-microservice/automating-your-microservice-008.png?featherlight=false&width=90pc)
9. Tại mục **Folder name**, nhập ```uploads```
* Click **Create folder**
![Deploy ImageManager Lambda Function](/images/4-extending-serverless-microservices/4.2-automating-your-microservice/automating-your-microservice-009.png?featherlight=false&width=90pc)
10. Click thư mục **uploads/**
![Deploy ImageManager Lambda Function](/images/4-extending-serverless-microservices/4.2-automating-your-microservice/automating-your-microservice-010.png?featherlight=false&width=90pc)
11. Click **Upload**
![Deploy ImageManager Lambda Function](/images/4-extending-serverless-microservices/4.2-automating-your-microservice/automating-your-microservice-011.png?featherlight=false&width=90pc)
12. Click **Add files**
* Chọn file **Puppy.jpg** chúng ta đã tải về
* CLick **Add files**
* Chọn file bất kỳ không phải ảnh
* Click **Upload**
![Deploy ImageManager Lambda Function](/images/4-extending-serverless-microservices/4.2-automating-your-microservice/automating-your-microservice-012.png?featherlight=false&width=90pc)
13. Click **Close**
![Deploy ImageManager Lambda Function](/images/4-extending-serverless-microservices/4.2-automating-your-microservice/automating-your-microservice-013.png?featherlight=false&width=90pc)
14. Chúng ta sẽ thấy tệp tin không phải hình ảnh chúng ta tải lên đã bị xóa.
* Click **idevelop-imagemanager-<YOUR_ACCOUNT_ID>**
![Deploy ImageManager Lambda Function](/images/4-extending-serverless-microservices/4.2-automating-your-microservice/automating-your-microservice-014.png?featherlight=false&width=90pc)
15. Click thư mục **processed**
![Deploy ImageManager Lambda Function](/images/4-extending-serverless-microservices/4.2-automating-your-microservice/automating-your-microservice-015.png?featherlight=false&width=90pc)
16. Chúng ta sẽ thấy hình ảnh thu nhỏ của tệp tin **Puppy.jpg**
![Deploy ImageManager Lambda Function](/images/4-extending-serverless-microservices/4.2-automating-your-microservice/automating-your-microservice-016.png?featherlight=false&width=90pc)
17. Truy cập [**Amazon CloudFormation Console**](https://console.aws.amazon.com/cloudformation/home).
* Click **Stacks**
* Nhập ```ImageManagerDemo``` vào thanh tìm kiếm và nhấn **Enter**
* Click tên của CloudFormation stack
![Deploy ImageManager Lambda Function](/images/4-extending-serverless-microservices/4.2-automating-your-microservice/automating-your-microservice-017.png?featherlight=false&width=90pc)
18. Click **Monitor**
* Click **View logs in CloudWatch**
![Deploy ImageManager Lambda Function](/images/4-extending-serverless-microservices/4.2-automating-your-microservice/automating-your-microservice-018.png?featherlight=false&width=90pc)
19. Click **Log stream** đầu tiên trong bảng **Log streams**
![Deploy ImageManager Lambda Function](/images/4-extending-serverless-microservices/4.2-automating-your-microservice/automating-your-microservice-019.png?featherlight=false&width=90pc)
20. Chúng ta hãy xem quá trình thực hiện như thế nào trong **CloudWatch log**
![Deploy ImageManager Lambda Function](/images/4-extending-serverless-microservices/4.2-automating-your-microservice/automating-your-microservice-020.png?featherlight=false&width=90pc)