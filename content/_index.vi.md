+++
title = "Tạo một Microservice"
date = 2021
weight = 1
chapter = false
+++
# Tạo một Microservice

#### Tổng quan

Trong phần này, bạn sẽ học cách tạo một hàm AWS Lambda được kích hoạt khi có tập tin được tải lên Amazon S3 Bucket. Bạn sẽ tải đoạn mã nguồn lên hàm Lambda, sau đó kết nối thủ công tới một trình kích hoạt sự kiện S3 để gọi hàm và xem log đầu ra. Sau đó, bạn sẽ chỉnh sửa lại code và thêm các chức năng cho phép nó xử lý các loại tập tin khác nhau - cụ thể là tạo hình thu nhỏ cho các hình ảnh JPG và xóa tất cả các loại tệp khác. Cuối cùng, bạn sẽ tạo một gói triển khai và tự động hóa việc triển khai hàm cũng như các trình kích hoạt liên quan và S3 bucket, sử dụng AWS CLI. 

![Architecture](/images/1-introduction/info.png?featherlight=false&width=90pc)

#### Nội dung:

1. [Giới thiệu](1-introduction/)
2. [Chuẩn bị](2-prepare/)
3. [Tạo một microservice](3-create-serverless-microservices/)
4. [Mở rộng Serverless Microservices](4-extending-serverless-microservices/)
5. [Cấu hình điều phối với CodeStar](5-use-codestar-orchestration/)
6. [Thử thách - Expose microservice API](6-challenge/)
7. [Dọn dẹp tài nguyên](7-cleanup/)