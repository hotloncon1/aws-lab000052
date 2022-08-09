+++
title = "Triển khai lại thông qua CI/CD pipeline "
date = 2020
weight = 2
chapter = false
pre = "<b>5.2. </b>"
+++
#### Triển khai lại thông qua CI/CD pipeline  

1. Trong **Command Prompt**, chuyển đường dẫn đến project **dev-flight-svc**
* Chạy câu lệnh dưới đây để tạo nhánh mới
```
git checkout -b "new-implementation"
```
![Deploy ImageManager Lambda Function](/images/5-use-codestar-orchestration/5.2-redeploy-through-cicd-pipeline/redeploy-through-cicd-pipeline-001.png?featherlight=false&width=60pc)

{{%attachments title="FlightSpecials file" style="orange" pattern="FlightSpecials.zip"/%}}

2. Tải file **FlightSpecials.zip** 
* Giải nén
* Copy toàn bộ nội dung của project **FlightSpecials** sau khi giải nén
3. Trong Eclipse IDE, nhấp chuột phải vào project **dev-flight-svc**. 
* Click **Show In** 
* Click **System Explorer**.
![Deploy ImageManager Lambda Function](/images/5-use-codestar-orchestration/5.2-redeploy-through-cicd-pipeline/redeploy-through-cicd-pipeline-002.png?featherlight=false&width=90pc)
4. Xóa nội dung trong hai thư mục **/src** và **/target** của project **dev-flight-svc** trước khi copy đè lên.
* Dán đè nội dung của project **FlightSpecials** vào thư mục đã mở trong bước 3
{{% notice warning %}} 
Nếu bạn không xóa nội dung hai thư mục **/src** và **/target** trước khi copy code mới vào thì quá trình build sẽ bị lỗi vì chúng ta không cấu hình cho **HelloWorldController / Handler**.
{{% /notice %}}
5. Trong Eclipse IDE, nhấp chuột phải vào project **dev-flight-svc**.
* Click **Maven**
* Click **Update Project...**
![Deploy ImageManager Lambda Function](/images/5-use-codestar-orchestration/5.2-redeploy-through-cicd-pipeline/redeploy-through-cicd-pipeline-003.png?featherlight=false&width=90pc)
6. Click **OK**
![Deploy ImageManager Lambda Function](/images/5-use-codestar-orchestration/5.2-redeploy-through-cicd-pipeline/redeploy-through-cicd-pipeline-004.png?featherlight=false&width=90pc)

{{% notice note %}} 
Là một phần của cài đặt microservice, chúng ta sử dụng VPC và gán một IAM Role mới cho hàm Lambda để cho phép nó thực hiện các tác vụ khác nhau. Khi CodeStar tạo project, nó tạo một IAM Role cấp đủ quyền cho CloudFormation để triển khai dịch vụ HelloWorld. Các quyền này không đủ cho các yêu cầu cao hơn, do đó, chúng ta cần thay đổi policy để gán quyền cho CloudFormation để mở rộng các quyền.
{{% /notice %}}

7. Truy cập vào [**AWS IAM Console**](https://console.aws.amazon.com/iamv2/).
* Click **Roles**.
* Nhập ```CodeStarWorker-dev-flight-svc-CloudFormation``` vào ô tìm kiếm
* Click **CodeStarWorker-dev-flight-svc-CloudFormation**
![Deploy ImageManager Lambda Function](/images/5-use-codestar-orchestration/5.2-redeploy-through-cicd-pipeline/redeploy-through-cicd-pipeline-005.png?featherlight=false&width=90pc)
8. Click **Add permissions**
* Click **Create inline policy**
![Deploy ImageManager Lambda Function](/images/5-use-codestar-orchestration/5.2-redeploy-through-cicd-pipeline/redeploy-through-cicd-pipeline-006.png?featherlight=false&width=90pc)
9. Click tab **JSON**
* Dán đoạn mã sau đây vào
```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Action": [
        "iam:GetRole",
        "iam:CreateRole",
        "iam:DeleteRole",
        "iam:PassRole",
        "iam:PutRolePolicy",
        "iam:DeleteRolePolicy",
        "lambda:ListTags",
        "lambda:TagResource",
        "lambda:UntagResource",
        "lambda:AddPermission",
        "ec2:DescribeSecurityGroups",
        "ec2:DescribeSubnets",
        "ec2:DescribeVpcs",
        "ec2:CreateNetworkInterface",
        "ec2:AttachNetworkInterface",
        "ec2:DescribeNetworkInterfaces"
      ],
      "Resource": "*",
      "Effect": "Allow"
    }
  ]
}
```
* Click **Review policy**
![Deploy ImageManager Lambda Function](/images/5-use-codestar-orchestration/5.2-redeploy-through-cicd-pipeline/redeploy-through-cicd-pipeline-007.png?featherlight=false&width=90pc)
10. Tại mục **Name**, nhập ```idevelopCodeStarCloudFormationPolicy```
* Click **Create policy**
![Deploy ImageManager Lambda Function](/images/5-use-codestar-orchestration/5.2-redeploy-through-cicd-pipeline/redeploy-through-cicd-pipeline-008.png?featherlight=false&width=90pc)

{{% notice info %}} 
Khi thêm vai trò **CodeStarWorker-dev-flight-svc-CloudFormation** các quyền trong **idevelopCodeStarCloudFormationPolicy** policy cho phép CLoudFormation hành động thay mặt bạn khi các triển khai thay đổi trong môi trường, bao gồm quyền cho phép hàm Lambda gắn với VPC nơi RDS instance lưu trữ trang web **TravelBuddy** được triển khai. Chúng cũng cho phép CloudFormation tạo một IAM Role mới mà hàm Lambda sử dụng để thực thi.
{{% /notice %}}

11. Tập tin CloudFormation template **template.yml** được cung cấp trong **FlightSpecials.zip** có một vài giá trị cần cập nhật trước khi tiến hành triển khai. Những giá trị này bao gồm **Subnet Ids**, **Security Group Ids** and the **RDS Instance Endpoint**.

* Truy cập [**Amazon EC2 console**](https://console.aws.amazon.com/ec2/).
* Click **Security Groups**.
* Nhập ```DBSecurityGroup``` vào ô tìm kiếm và nhấn **Enter**
* Lưu lại giá trị **Security Group Id** của **DBSecurityGroup**
![Deploy ImageManager Lambda Function](/images/5-use-codestar-orchestration/5.2-redeploy-through-cicd-pipeline/redeploy-through-cicd-pipeline-009.png?featherlight=false&width=90pc)
12. Truy cập [**Amazon VPC console**](https://console.aws.amazon.com/vpc/).
* Click **Subnets**.
* Nhập ```Module3/DevAxNetworkVPC/privateSubnet``` vào ô tìm kiếm và nhấn **Enter**
* Lưu lại giá trị **Subnet ID** của subnet **Module3/DevAxNetworkVPC/privateSubnet1** và subnet **Module3/DevAxNetworkVPC/privateSubnet2**
![Deploy ImageManager Lambda Function](/images/5-use-codestar-orchestration/5.2-redeploy-through-cicd-pipeline/redeploy-through-cicd-pipeline-010.png?featherlight=false&width=90pc)
13. Truy cập [AWS CloudFormation Console](https://console.aws.amazon.com/cloudformation/).
* Nhập **aws-stack-for-Devax-lab03** vào ô tìm kiếm và nhấn **Enter**.
* Click **aws-stack-for-Devax-lab03**.
![Deploy ImageManager Lambda Function](/images/5-use-codestar-orchestration/5.2-redeploy-through-cicd-pipeline/redeploy-through-cicd-pipeline-011.png?featherlight=false&width=90pc)
14. Click **Output**
* Lưu lại giá trị **RDSEndpoint**
![Deploy ImageManager Lambda Function](/images/5-use-codestar-orchestration/5.2-redeploy-through-cicd-pipeline/redeploy-through-cicd-pipeline-012.png?featherlight=false&width=90pc)
15. Trong Eclipse IDE, mở file **template.yml**
* Thay **sg-\<REPLACE\>** bằng giá trị **Security Group Id** của **DBSecurityGroup** đã lưu trong bước 11
* Thay **subnet-\<REPLACE\>** lần lượt bằng giá trị **Subnet ID** của subnet **Module3/DevAxNetworkVPC/privateSubnet1** và subnet **Module3/DevAxNetworkVPC/privateSubnet2** đã lưu trong bước 12
* Thay **\<RDSEndpoint\>** bằng giá trị **RDSEndpoint** đã lưu trong bước 14
* Lưu lại
![Deploy ImageManager Lambda Function](/images/5-use-codestar-orchestration/5.2-redeploy-through-cicd-pipeline/redeploy-through-cicd-pipeline-013.png?featherlight=false&width=90pc)