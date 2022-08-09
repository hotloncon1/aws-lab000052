+++
title = "Cài đặt S3 buckets"
date = 2020
weight = 4
chapter = false
pre = "<b>2.4. </b>"
+++
#### Cài đặt S3 buckets

Bạn cần cài đặt 2 buckets. Một bucket sẽ chứa hàm lambda được tải lên và một bucket được sử dụng để lưu trữ hình ảnh.
{{% notice note %}} 
Bạn có thể sử dụng AWS Console hoặc dùng command line để tạo S3 buckets.
{{% /notice %}}
1. Truy cập máy ảo window, mở command prompt
2. Chạy lệnh dưới đây để tạo một S3 bucket để lưu hàm Lambda và một bucket để chứa hình ảnh tải lên
```
aws s3 mb s3://idevelop-sourcecode-<yourinitials> --region <region> --profile devaxacademy
aws s3 mb s3://idevelop-imagemanager-<yourinitials> --region <region> --profile devaxacademy
```
{{% notice note %}} 
Thay ```<yourinitials>``` bằng tên của bạn để tạo ra tên duy nhất cho S3 bucket.\
Thay ```<region>``` bằng **region** bạn đang làm bài thực hành này.
{{% /notice %}}
![Setup S3 bucket](/images/2-prepare/2.4-setups3/setups3-001.png?featherlight=false&width=60pc)
3. Truy cập [**AWS S3 Console**](https://s3.console.aws.amazon.com/s3/).
* Gõ ```idevelop``` vào ô tìm kiếm, chúng ta sẽ thấy 2 S3 bucket
![Setup S3 bucket](/images/2-prepare/2.4-setups3/setups3-002.png?featherlight=false&width=90pc)

