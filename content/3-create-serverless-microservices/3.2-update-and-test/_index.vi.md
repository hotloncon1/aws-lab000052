+++
title = "Tải lên và kiểm tra hàm Lambda trong AWS Lambda "
date = 2020
weight = 2
chapter = false
pre = "<b>3.2. </b>"
+++
#### Tải lên và kiểm tra hàm Lambda trong AWS Lambda 
1. Nhấp chuột phải bên trong cửa sổ chứa mã nguồn của **LambdaFunctionHandler.java** trong **src/main/java/idevelop.lambda.s3handler**.
* Click **AWS Lambda**
* Click **Run function on AWS Lambda…**
![Upload and test Lambda function on AWS Lambda](/images/3-create-serverless-microservices/3.2-update-and-test/update-and-test-001.png?featherlight=false&width=90pc)
2. Click **Upload now**
![Upload and test Lambda function on AWS Lambda](/images/3-create-serverless-microservices/3.2-update-and-test/update-and-test-002.png?featherlight=false&width=90pc)
3. Chọn region đang thực hiện bài thực hành 
* Chọn **Create a new Lambda function**.
* Nhập tên hàm Lambda là ```TestLambda```
* Click **Next**
![Upload and test Lambda function on AWS Lambda](/images/3-create-serverless-microservices/3.2-update-and-test/update-and-test-003.png?featherlight=false&width=90pc)
4. Tại mục **description**, nhập ```Test AWS Lambda function triggered by S3 upload```
* Tại mục **IAM role**, chọn **LambdaRole** được tạo tự động trong phần cấu hình bài thực hành.
* Tại mục **S3 bucket**, chọn S3 bucket có tên bắt đầu bằng **idevelop-sourcecode-** chúng ta đã tạo từ trước để chứa mã nguồn Lambda.
* Chọn **Finish** để tải hàm Lambda lên AWS Account. 
![Upload and test Lambda function on AWS Lambda](/images/3-create-serverless-microservices/3.2-update-and-test/update-and-test-004.png?featherlight=false&width=90pc)
5. Việc tải lên sẽ tốn một vài phút. 
* Truy cập [**AWS Lambda Console**](https://console.aws.amazon.com/lambda) 
* Click **Functions**. 
* Nhập ```TestLambda``` vào ô tìm kiếm và nhấn **Enter** .Chúng ta sẽ thấy hàm **TestLambda** vừa được tải lên.
* Click hàm **TestLambda**
![Upload and test Lambda function on AWS Lambda](/images/3-create-serverless-microservices/3.2-update-and-test/update-and-test-005.png?featherlight=false&width=90pc)
6. Click **Add Triggers** 
![Upload and test Lambda function on AWS Lambda](/images/3-create-serverless-microservices/3.2-update-and-test/update-and-test-006.png?featherlight=false&width=90pc)
7. Tại mục **Trigger configuration**, chọn **S3**
* Tại mục **Bucket**, chọn bucket có tên bắt đầu bằng **idevelop-imagemanager-** mà bạn đã tạo để chứa hình ảnh tải lên
* Tại mục **Event type** chọn **All Object create events**
* Tại mục **Prefix** nhập ```uploads/```. Không đặt giá trị **Suffix**.
* Click **I acknowledge that using the same S3 bucket for both input and output is not recommended and that this configuration can cause recursive invocations, increased Lambda usage, and increased costs.**
* Click **Add**
![Upload and test Lambda function on AWS Lambda](/images/3-create-serverless-microservices/3.2-update-and-test/update-and-test-007.png?featherlight=false&width=90pc)
8. Bây giờ chúng ta sẽ cùng kiểm tra hàm Lambda. Truy cập [**AWS S3 Console**](https://s3.console.aws.amazon.com/s3/)
* Nhập ```idevelop-imagemanager``` vào ô tìm kiếm
* Click S3 bucket có tên bắt đầu bằng **idevelop-imagemanager**
![Upload and test Lambda function on AWS Lambda](/images/3-create-serverless-microservices/3.2-update-and-test/update-and-test-008.png?featherlight=false&width=90pc)
9. Click **Create folder**
![Upload and test Lambda function on AWS Lambda](/images/3-create-serverless-microservices/3.2-update-and-test/update-and-test-009.png?featherlight=false&width=90pc)
10. Trong trang **Create folder**
* Tại mục **Folder name**, điền ```uploads```
* Click **Create folder**
![Upload and test Lambda function on AWS Lambda](/images/3-create-serverless-microservices/3.2-update-and-test/update-and-test-010.png?featherlight=false&width=90pc)
11. Click **uploads/** để mở folder **uploads**
![Upload and test Lambda function on AWS Lambda](/images/3-create-serverless-microservices/3.2-update-and-test/update-and-test-011.png?featherlight=false&width=90pc)

{{%attachments title="Puppy image" style="orange" pattern="Puppy.jpg"/%}}

12. Tải tệp tin **Puppy.jpg**

13. Click **Upload**
![Upload and test Lambda function on AWS Lambda](/images/3-create-serverless-microservices/3.2-update-and-test/update-and-test-012.png?featherlight=false&width=90pc)
14. Click **Add files**
* Chọn file **Puppy.jpg** đã tải trong bước 12
* Click **Upload**
![Upload and test Lambda function on AWS Lambda](/images/3-create-serverless-microservices/3.2-update-and-test/update-and-test-013.png?featherlight=false&width=90pc)
15. Khi việc tải lên được hoàn tất
* Truy cập [**AWS Lambda Console**](https://console.aws.amazon.com/lambda)
* Click **Funtions**
* Nhập ```TestLambda``` vào ô tìm kiếm và nhấn **Enter**
* Click **TestLambda**
![Upload and test Lambda function on AWS Lambda](/images/3-create-serverless-microservices/3.2-update-and-test/update-and-test-014.png?featherlight=false&width=90pc)
16. Click tab **Monitor**
* Bạn sẽ thấy 2 lệnh gọi và thời lượng của chúng trong biểu đồ. Bạn thấy 2 lệnh gọi - một là tạo thư mục uploads và một là tải lên hình ảnh Puppy.jpg
![Upload and test Lambda function on AWS Lambda](/images/3-create-serverless-microservices/3.2-update-and-test/update-and-test-015.png?featherlight=false&width=90pc)
17. Click **View logs in CloudWatch** để mở CloudWatch logs của hàm Lambda này. 
![Upload and test Lambda function on AWS Lambda](/images/3-create-serverless-microservices/3.2-update-and-test/update-and-test-016.png?featherlight=false&width=90pc)
* Chúng ta sẽ thấy có log streams được ghi lại
![Upload and test Lambda function on AWS Lambda](/images/3-create-serverless-microservices/3.2-update-and-test/update-and-test-017.png?featherlight=false&width=90pc)
* Click vào log steam đầu tiên để xem thông tin chi tiết
![Upload and test Lambda function on AWS Lambda](/images/3-create-serverless-microservices/3.2-update-and-test/update-and-test-018.png?featherlight=false&width=90pc)
{{% notice note %}} 
Bạn sẽ thấy **CONTENT TYPE output** khác nhau ở 2 sự kiện này. Trong sự kiện đầu tiên, CONTENT TYPE là **application/x-directory** và ở sự kiện thứ 2 là image/jpeg. Trong mã nguồn mẫu của hàm Lambda đã cung cấp, chỉ ghi logs CONTENT TYPE của tập tin mà không thực hiện hành động nào khác. Chúng ta sẽ thay đổi đoạn code này trong các phần tiếp theo.
{{% /notice %}}
18. Tải một tập tin bất kỳ không phải là hình ảnh lên S3 bucket và quan sát log (làm tương tự bước 13 và 14).

![Upload and test Lambda function on AWS Lambda](/images/3-create-serverless-microservices/3.2-update-and-test/update-and-test-019.png?featherlight=false&width=90pc)
* Quan sát log (làm tương tự bước 15, 16 và 17)
![Upload and test Lambda function on AWS Lambda](/images/3-create-serverless-microservices/3.2-update-and-test/update-and-test-020.png?featherlight=false&width=90pc)