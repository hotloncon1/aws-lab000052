+++
title = "Tạo và kiểm tra hàm Lambda tại cục bộ"
date = 2020
weight = 1
chapter = false
pre = "<b>3.1. </b>"
+++
#### Tạo và kiểm tra hàm Lambda tại cục bộ
1. Mở **Eclipse IDE** trong máy ảo Windows.
* Tìm biểu tượng **AWS**, nhấp vào mũi tên thả xuống
* Click **New AWS Lambda Java Project…**
![Create and test Lambda function locally](/images/3-create-serverless-microservices/3.1-create-lambda-function/create-lambda-function-001.png?featherlight=false&width=90pc)
2. Trong hộp thoại **New AWS Lambda Maven Project**
* Tại mục **Project name**, gõ ```TestLambda```
* Tại mục **Group ID**, gõ ```idevelop.lambda```
* Tại mục **Artifact ID**, gõ ```s3handler```
* Click **Finish**
![Create and test Lambda function locally](/images/3-create-serverless-microservices/3.1-create-lambda-function/create-lambda-function-002.png?featherlight=false&width=90pc)
3. Chúng ta cần cập nhật tập tin pom.xml mà Maven sử dụng lên phiên bản Mockito mới hơn
* Mở project **TestLambda**
* Mở file **pom.xml**
* Tìm phần **dependency** có **artifactId** là **mockito-core** và thay phần **version** thành ```3.3.3```
* Lưu lại
![Create and test Lambda function locally](/images/3-create-serverless-microservices/3.1-create-lambda-function/create-lambda-function-003.png?featherlight=false&width=90pc)
4. Chạy **JUnit Test**
* Nhấp chuột phải vào thư mục root của project **TestLambda**. Click **Run As**
* Click **JUnit Test**
![Create and test Lambda function locally](/images/3-create-serverless-microservices/3.1-create-lambda-function/create-lambda-function-004.png?featherlight=false&width=90pc)
5. Bạn sẽ thấy kết quả đầu ra từ hàm lambda, như thể nó được kích hoạt bởi một tập tin được tải lên S3. Các tham số cho bài test được cung cấp trong test resource, ở dạng JSON payload giống với payload mà môi trường Amazon sẽ gửi đến hàm Lambda, khi S3 bucket được liên kết với hàm Lambda này nhận được tập tin được tải lên. 
![Create and test Lambda function locally](/images/3-create-serverless-microservices/3.1-create-lambda-function/create-lambda-function-005.png?featherlight=false&width=90pc)
{{% notice note %}} 
Bạn có thể sẽ thấy một vài cảnh báo liên quan tới profile name. Bạn có thể bỏ qua cảnh báo này trong bài thực hành này.
{{% /notice %}}
6. Để xem kết quả đầu ra của phần JUnit test, click tab **JUnit**
![Create and test Lambda function locally](/images/3-create-serverless-microservices/3.1-create-lambda-function/create-lambda-function-006.png?featherlight=false&width=90pc)
7. Kiểm tra tập tin **S3-event.put.json** có đường dẫn **TestLambda/src/test/resources/s3-event.put.json**. Tập tin **S3-event.put.json** chứa những schema và giá trị mà chúng ta sẽ sử dụng cho bài thực hành này.
![Create and test Lambda function locally](/images/3-create-serverless-microservices/3.1-create-lambda-function/create-lambda-function-007.png?featherlight=false&width=90pc)
{{% notice note %}} 
Bạn có thể sẽ thấy một cảnh báo liên quan tới việc thiếu node. Bạn có thể bỏ qua cảnh báo này bằng cách click **OK**
{{% /notice %}}

#### Cập nhật code được cung cấp để xử lý các URL encoded keys
Mã nguồn được cung cấp không quan tâm tới việc mã hóa được áp dụng cho key name được cung cấp trong S3 event khi nó được gửi tới hàm Lambda, do đó nếu bạn tải một tập tin để kiểm tra, và tập tin chứa khoảng trắng hoặc dấu cấu thì chuỗi này cần được decode trước khi sử dụng.

8. Mở **LambdaFunctionHandler.java** có đường dẫn **src/main/java/idevelop.lambda.s3handler/LambdaFunctionHandler.java**
* Thêm đoạn code sau vào sau dòng 28 và lưu lại
```
try
{
  key = java.net.URLDecoder.decode(key, "UTF-8");
}
catch(Exception ex)
{
  context.getLogger().log("Could not decode URL for keyname... continuing...");
}
```
![Create and test Lambda function locally](/images/3-create-serverless-microservices/3.1-create-lambda-function/create-lambda-function-008.png?featherlight=false&width=90pc)