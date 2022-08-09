+++
title = "Cập nhật target region cho API "
date = 2020
weight = 3
chapter = false
pre = "<b>5.3. </b>"
+++
#### Cập nhật target region cho API 

Tập tin **swagger.yml** định nghĩa API được dùng cho microservice thông qua Amazon API Gateway. Chúng cần được cập nhật chi tiết AWS Account ID và target AWS Region trước khi triển khai microservice.
1. Trong Eclipse IDE, mở tập tin **swagger.yml**
* Nhấn tổ hợp phím **Ctrl+F**
* Tại mục **Find**, điền ```<REGION>```
* Tại mục **Replace with**, điền **Region** của bạn  
* Click **Replace All** để thay thế
![Deploy ImageManager Lambda Function](/images/5-use-codestar-orchestration/5.3-update-target-region/update-target-region-001.png?featherlight=false&width=90pc)
2. Làm tương tự bước 1 để thay thế ```<ACCOUNTID>``` bằng **AWS Account Id** của bạn
* Lưu lại
3. Tromg Command Prompt, chạy câu lệnh dưới đây để cấu hình email và username cho git
```
C:\Users\Administrator\git\dev-flight-svc>git config --global user.name "awsstudent"
C:\Users\Administrator\git\dev-flight-svc>git config --global user.email "<YOUR_EMAIL>"
```
{{% notice note %}} 
Thay **<YOUR_EMAIL>** bằng email của bạn
{{% /notice %}}
![Deploy ImageManager Lambda Function](/images/5-use-codestar-orchestration/5.3-update-target-region/update-target-region-002.png?featherlight=false&width=60pc)
4. Chạy lệnh dưới đây để xem lại thay đổi chưa được commit
```
git status
```
![Deploy ImageManager Lambda Function](/images/5-use-codestar-orchestration/5.3-update-target-region/update-target-region-003.png?featherlight=false&width=60pc)
5. Chạy lệnh dưới đây để thêm những tập tin thay đổi
```
git add .
git commit -m "Baseline implementation"
```
![Deploy ImageManager Lambda Function](/images/5-use-codestar-orchestration/5.3-update-target-region/update-target-region-004.png?featherlight=false&width=60pc)
6. Chạy lệnh dưới đây để trở về nhánh **master**
```
git checkout master
```
![Deploy ImageManager Lambda Function](/images/5-use-codestar-orchestration/5.3-update-target-region/update-target-region-005.png?featherlight=false&width=60pc)
7. Chạy lệnh dưới đây để merge thay đổi từ nhánh **new-implementation** vào nhánh **master**
```
git merge new-implementation
```
![Deploy ImageManager Lambda Function](/images/5-use-codestar-orchestration/5.3-update-target-region/update-target-region-006.png?featherlight=false&width=60pc)
8. Trong Eclipse IDE, nhấp chuột phải vào project **dev-flight-svc** 
* Click **Team** 
* Click **Push to origin**. 
![Deploy ImageManager Lambda Function](/images/5-use-codestar-orchestration/5.3-update-target-region/update-target-region-007.png?featherlight=false&width=90pc)
9. Click **Close**
![Deploy ImageManager Lambda Function](/images/5-use-codestar-orchestration/5.3-update-target-region/update-target-region-008.png?featherlight=false&width=90pc)
10. Truy cập [**Amazon EC2 console**](https://console.aws.amazon.com/ec2/).
* Click **Security Groups**.
* Nhập ```DBSecurityGroup``` vào thanh tìm kiếm và nhấn **Enter**
* Chọn **DBSecurityGroup**
* Click tab **Inbound rules**
* Click **Edit inbound rules**
![Deploy ImageManager Lambda Function](/images/5-use-codestar-orchestration/5.3-update-target-region/update-target-region-009.png?featherlight=false&width=90pc)
11. Trong phần **Source**, chọn **DBSecurityGroup**
* Click **Save rules**
![Deploy ImageManager Lambda Function](/images/5-use-codestar-orchestration/5.3-update-target-region/update-target-region-010.png?featherlight=false&width=90pc)
12. Truy cập [**AWS CodeStar Console**](https://console.aws.amazon.com/codesuite/codestar/home).
* Click **Projects**
* Click **dev-flight-svc**
![Deploy ImageManager Lambda Function](/images/5-use-codestar-orchestration/5.3-update-target-region/update-target-region-011.png?featherlight=false&width=90pc)
13. Click **View application**
![Deploy ImageManager Lambda Function](/images/5-use-codestar-orchestration/5.3-update-target-region/update-target-region-012.png?featherlight=false&width=90pc)
14. Khi trang được mở, bạn sẽ thấy một thông báo lỗi **{“message”:“Missing Authentication Token”}**. Điều này xảy ra vì bạn đang cố gắng truy cập vào gốc của API, thay vì một microservice cụ thể. Chỉnh sửa URL, thêm **flightspecials** vào cuối URL
![Deploy ImageManager Lambda Function](/images/5-use-codestar-orchestration/5.3-update-target-region/update-target-region-013.png?featherlight=false&width=90pc)
15. Chúng ta sẽ được kết quả như sau
![Deploy ImageManager Lambda Function](/images/5-use-codestar-orchestration/5.3-update-target-region/update-target-region-014.png?featherlight=false&width=90pc)