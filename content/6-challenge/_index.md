+++
title = "Challenge - Expose the microservice API"
date = 2020
weight = 6
chapter = false
pre = "<b>6. </b>"
+++
#### Challenge - Expose the microservice API


{{%attachments title="HotelSpecial class" style="orange" pattern="HotelSpecial.java"/%}}

1. Copy the **HotelSpecial.java** file to the folder whose path is **…/src/main/java/devlounge/model** in the **dev-flight-svc** project
![Deploy ImageManager Lambda Function](/images/6-challenge/challenge-001.png?featherlight=false&width=90pc)

{{%attachments title="template file" style="orange" pattern="template.yml"/%}}

2. Replace the contents of the **template.yml** file in the **dev-flight-svc** project with the content of the above **template.yml** file
* Do the same as the step 11, 12, 13, 14 and 15 in the section 5.2 to update the **template.yml** file in the **dev-flight-svc** project

{{%attachments title="swagger file" style="orange" pattern="swagger.yml"/%}}

3. Thay nội dung của file **swagger.yml** in the **dev-flight-svc** project bằng nội dung của file **swagger.yml** bên trên
* Do the same as the step 1 and 2 in the section 5.3 to update the **swagger.yml** file in the **dev-flight-svc** project
4. Trong **Command Prompt**, chuyển đường dẫn đến project **dev-flight-svc**
* Execute the below command to add in the changed files
```
git add .
git commit -m "Update implementation"
```
5. In the Eclipse IDE, right-click on the **dev-flight-svc** project
* Click **Team** 
* Click **Push to origin**. 
![Deploy ImageManager Lambda Function](/images/6-challenge/challenge-002.png?featherlight=false&width=90pc)
6. Click **Close**
![Deploy ImageManager Lambda Function](/images/6-challenge/challenge-003.png?featherlight=false&width=90pc)
7. Go to [**AWS CodeStar Console**](https://console.aws.amazon.com/codesuite/codestar/home).
* Click **Projects**
* Click **dev-flight-svc**
![Deploy ImageManager Lambda Function](/images/6-challenge/challenge-004.png?featherlight=false&width=90pc)
8. Click **View application**
![Deploy ImageManager Lambda Function](/images/6-challenge/challenge-005.png?featherlight=false&width=90pc)
9. Add **hotelspecials** to the end of the URL and press **Enter**
* We will see the following result
![Deploy ImageManager Lambda Function](/images/6-challenge/challenge-006.png?featherlight=false&width=90pc)

#### Bài tập nâng cao tùy chọn
5. Sửa đổi mã nguồn từ phần 3 để phát hiện các loại file khác nhau và xử lý chúng theo cách tùy chọn (ví dụ, chặn và di chuyển chúng tới một thư mục khác)
6. Tạo một hàm Lambda mới thực hiện các task mỗi phút một lần. Task có thể đơn giản như ghi kết quả đầu ra vào CloudWatch Logs.