+++
title = "Thử thách - Expose microservice API"
date = 2020
weight = 6
chapter = false
pre = "<b>6. </b>"
+++
#### Thử thách - Expose microservice API

{{%attachments title="HotelSpecial class" style="orange" pattern="HotelSpecial.java"/%}}

1. Sao chép tập tin **HotelSpecial.java** vào đường dẫn **…/src/main/java/devlounge/model** trong project **dev-flight-svc**
![Deploy ImageManager Lambda Function](/images/6-challenge/challenge-001.png?featherlight=false&width=90pc)

{{%attachments title="template file" style="orange" pattern="template.yml"/%}}

2. Thay nội dung của file **template.yml** trong project **dev-flight-svc** bằng nội dung của file **template.yml** bên trên
* Làm tương tự bước 11, 12, 13, 14 và 15 trong phần 5.2 để cập nhật các giá trị trong file **template.yml** trong project **dev-flight-svc**

{{%attachments title="swagger file" style="orange" pattern="swagger.yml"/%}}

3. Thay nội dung của file **swagger.yml** trong project **dev-flight-svc** bằng nội dung của file **swagger.yml** bên trên
* Làm tương tự bước 1 và 2 trong phần 5.3 để cập nhật các giá trị của file **swagger.yml** trong project **dev-flight-svc**
4. Trong **Command Prompt**, chuyển đường dẫn đến project **dev-flight-svc**
* Chạy câu lệnh dưới đây để commit những thay đổi
```
git add .
git commit -m "Update implementation"
```
5. Trong Eclipse IDE, nhấp chuột phải vào project **dev-flight-svc** 
* Click **Team** 
* Click **Push to origin**. 
![Deploy ImageManager Lambda Function](/images/6-challenge/challenge-002.png?featherlight=false&width=90pc)
6. Click **Close**
![Deploy ImageManager Lambda Function](/images/6-challenge/challenge-003.png?featherlight=false&width=90pc)
7. Truy cập [**AWS CodeStar Console**](https://console.aws.amazon.com/codesuite/codestar/home).
* Click **Projects**
* Click **dev-flight-svc**
![Deploy ImageManager Lambda Function](/images/6-challenge/challenge-004.png?featherlight=false&width=90pc)
8. Click **View application**
![Deploy ImageManager Lambda Function](/images/6-challenge/challenge-005.png?featherlight=false&width=90pc)
9. Thêm **hotelspecials** vào cuối URL và nhấn **Enter**
* Chúng ta sẽ được kết quả như sau.
![Deploy ImageManager Lambda Function](/images/6-challenge/challenge-006.png?featherlight=false&width=90pc)

#### Bài tập nâng cao tùy chọn
5. Sửa đổi mã nguồn từ phần 3 để phát hiện các loại file khác nhau và xử lý chúng theo cách tùy chọn (ví dụ, chặn và di chuyển chúng tới một thư mục khác)
6. Tạo một hàm Lambda mới thực hiện các task mỗi phút một lần. Task có thể đơn giản như ghi kết quả đầu ra vào CloudWatch Logs.