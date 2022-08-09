+++
title = "Tạo nhánh mới"
date = 2020
weight = 1
chapter = false
pre = "<b>5.1. </b>"
+++
#### Tạo nhánh mới

Bây giờ bạn đã có kinh nghiệm thực tế về việc tạo và triển khai các hàm AWS Lambda, đã đến lúc quay lại ứng dụng TravelBuddy monolithic của chúng ta và triển khai một microservice trên AWS Lambda được quản lý thông qua CI/CD pipeline được tạo bằng AWS CodeStar.

1. Truy cập [**AWS CodeStar Console**](https://console.aws.amazon.com/codesuite/codestar/home).
* Click **Projects**
* Click **Create project**
{{% notice note %}} 
Click **Create service role** nếu đây là lần đầu bạn truy cập vào dịch vụ **AWS CodeStar**.
{{% /notice %}}
![Deploy ImageManager Lambda Function](/images/5-use-codestar-orchestration/5.1-create-new-branch/create-new-branch-001.png?featherlight=false&width=90pc)
2. Trong phần **Templates**, chọn **Java**, **Web Service** và **AWS Lambda**
* Chọn **Java Spring**
* Click **Next**
![Deploy ImageManager Lambda Function](/images/5-use-codestar-orchestration/5.1-create-new-branch/create-new-branch-002.png?featherlight=false&width=90pc)
3. Tại mục **Project name**, nhập ```dev-flight-svc```
* Chọn **Code Commit**
* Click **Next**
![Deploy ImageManager Lambda Function](/images/5-use-codestar-orchestration/5.1-create-new-branch/create-new-branch-003.png?featherlight=false&width=90pc)
4. Tại trang **Review**, click **Create project**
![Deploy ImageManager Lambda Function](/images/5-use-codestar-orchestration/5.1-create-new-branch/create-new-branch-004.png?featherlight=false&width=90pc)
5. Thêm tài khoản **awsstudent** vào Team với vai trò **Owner**.
* Click **Team**
* Click **Add team member**
![Deploy ImageManager Lambda Function](/images/5-use-codestar-orchestration/5.1-create-new-branch/create-new-branch-005.png?featherlight=false&width=90pc)
6.  Trong phần **Team member details**
* Tại mục **User**, chọn **awsstudent**
* Tại mục **Email address**, điền email của bạn
* Tại mục **Project role**, chọn **Owner**
* CLick **Allow SSH access to project instances.**
* Click **Add team member**
![Deploy ImageManager Lambda Function](/images/5-use-codestar-orchestration/5.1-create-new-branch/create-new-branch-006.png?featherlight=false&width=90pc)
7. Kiểm tra team member được add vào thành công
![Deploy ImageManager Lambda Function](/images/5-use-codestar-orchestration/5.1-create-new-branch/create-new-branch-007.png?featherlight=false&width=90pc)
8. Truy cập [AWS CloudFormation Console](https://console.aws.amazon.com/cloudformation/).
* Nhập **aws-stack-for-Devax-lab03** vào ô tìm kiếm và nhấn **Enter**.
* Click **aws-stack-for-Devax-lab03**.
![Deploy ImageManager Lambda Function](/images/5-use-codestar-orchestration/5.1-create-new-branch/create-new-branch-008.png?featherlight=false&width=90pc)
9. Click **Output**
* Lưu lại giá trị **GitPassword** và **GitUserName**
![Deploy ImageManager Lambda Function](/images/5-use-codestar-orchestration/5.1-create-new-branch/create-new-branch-009.png?featherlight=false&width=90pc)
10. Trong Eclipse IDE, tìm biểu tượng **AWS** và nhấp vào mũi tên thả xuống
* Click **Import AWS CodeStar Project**
![Deploy ImageManager Lambda Function](/images/5-use-codestar-orchestration/5.1-create-new-branch/create-new-branch-010.png?featherlight=false&width=90pc)
11. Chọn **Region** chúng ta đang sử dụng
* Chọn ```dev-flight-svc```
* Điền thông tin đã lưu trong bước 9 vào phần **User name** và **Password**
* Click **Next**
![Deploy ImageManager Lambda Function](/images/5-use-codestar-orchestration/5.1-create-new-branch/create-new-branch-011.png?featherlight=false&width=90pc)
12. Click **OK**, bỏ qua lỗi **org.eclipse.egit.ui.internal.repository.tree.RepositoryTreeNodeType.getIcon()Lorg/eclipse/swt/graphics/Image;**
* Chọn branch **master**
* Click **Next**.
![Deploy ImageManager Lambda Function](/images/5-use-codestar-orchestration/5.1-create-new-branch/create-new-branch-012.png?featherlight=false&width=90pc)
13. Click **Finish**
![Deploy ImageManager Lambda Function](/images/5-use-codestar-orchestration/5.1-create-new-branch/create-new-branch-013.png?featherlight=false&width=90pc)
14. Click **No** để bỏ qua thiết lập password hint.
![Deploy ImageManager Lambda Function](/images/5-use-codestar-orchestration/5.1-create-new-branch/create-new-branch-014.png?featherlight=false&width=90pc)
15. Hãy dành một ít thời gian để xem cấu trúc project trước khi tiếp tục. Microservice HelloWorld biểu diễn một xử lý đơn giản, trả về trang web Hello World khi được gọi.