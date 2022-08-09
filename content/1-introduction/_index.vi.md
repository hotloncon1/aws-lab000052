+++
title = "Giới thiệu"
date = 2020
weight = 1
chapter = false
pre = "<b>1. </b>"
+++
#### Giới thiệu

Trong phần này, bạn sẽ học cách tạo một hàm AWS Lambda được kích hoạt khi có tập tin được tải lên Amazon S3 Bucket. Bạn sẽ tải đoạn mã nguồn lên hàm Lambda, sau đó kết nối thủ công tới một trình kích hoạt sự kiện S3 để gọi hàm và xem log đầu ra. Sau đó, bạn sẽ chỉnh sửa lại code và thêm các chức năng cho phép nó xử lý các loại tập tin khác nhau - cụ thể là tạo hình thu nhỏ cho các hình ảnh JPG và xóa tất cả các loại tệp khác. Cuối cùng, bạn sẽ tạo một gói triển khai và tự động hóa việc triển khai hàm cũng như các trình kích hoạt liên quan và S3 bucket, sử dụng AWS CLI. 

![Architecture](/images/1-introduction/info.png?featherlight=false&width=90pc)

Phiên bản cuối cùng của hàm AWS Lambda thực hiện những nhiệm vụ sau đây:
* Đầu tiên, tải một tập tin lên S3 bucket.
* Amazon S3 kích hoạt hàm AWS Lambda được liên kết với hành động PutObject và cung cấp metadata để mô tả tập tin.
* Hàm Lambda kiểm tra loại nội dung của tập tin và nếu nó không phải là tập tin **image/jpeg**, tập tin sẽ bị xóa. Nếu tập tin là tập tin **image/jpeg**, đoạn code sẽ sinh ra một tập tin thu nhỏ của hình ảnh và lưu hình thu nhỏ này ở một thư mục khác trong cùng một bucket. 

Sau đó, bạn sẽ quay trở lại với ứng dụng web TravelBuddy và triển khai các phần monolithic codebase như một microservices độc lập, sử dụng AWS CodeStar để tạo CI/CD pipeline.

![Architecture](/images/1-introduction/monolithcallingmicro.png?featherlight=false&width=90pc)

#### Nội dung
Sau khi hoàn thành bài thực hành, bạn sẽ có thể:
* Sử dụng Eclipse IDE để tạo một **AWS Lambda function** và triển khai.
* Chỉnh sửa mã nguồn hàm Java Lambda đã được cung cấp để thay đổi kích thước hình ảnh dưới dạng hình thu nhỏ hoặc xóa tập tin không phải hình ảnh và kết nối trình kích hoạt vào S3 bucket để kiểm tra tính năng.
* Sử dụng **AWS Serverless Application Model (SAM)** và **AWS CloudFormation** để tạo template nhằm tự động hóa việc triển khai hàm Lambda, S3 trigger và S3 bucket.
* Xác định microservice candidate trong monolithic codebase và tạo dự án AWS CodeStar để quản lý CI/CD pipeline cho microservice đó được lưu trữ trong AWS Lambda.

#### Kiến thức kỹ thuật cần có
Để có thể hoàn thành bài thực hành này, bạn nên làm quen với:
* Những điều hướng cơ bản của AWS Management Console.
* Chỉnh sửa tập lệnh bằng trình soạn thảo.
* Sử dụng Eclipse IDE và ngôn ngữ lập trình Java

#### Môi trường
Sơ đồ sau miêu tả các tài nguyên được triển khai trong bài thực hành này.
![Architecture](/images/1-introduction/architecture.png?featherlight=false&width=50pc)