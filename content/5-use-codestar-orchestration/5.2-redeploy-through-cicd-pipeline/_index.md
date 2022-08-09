+++
title = "Redeploy through the CI/CD pipeline"
date = 2020
weight = 2
chapter = false
pre = "<b>5.2. </b>"
+++
#### Redeploy through the CI/CD pipeline


1. In the **Command Prompt**, navigate to the directory of the **dev-flight-svc** project
* Execute the following command to create a new branch
```
git checkout -b "new-implementation"
```
![Deploy ImageManager Lambda Function](/images/5-use-codestar-orchestration/5.2-redeploy-through-cicd-pipeline/redeploy-through-cicd-pipeline-001.png?featherlight=false&width=60pc)

{{%attachments title="FlightSpecials file" style="orange" pattern="FlightSpecials.zip"/%}}

2. Download the **FlightSpecials.zip** file
* Extract
* Copy the contents of the **FlightSpecials** extracted project
3. In the Eclipse IDE, right-click on the **dev-flight-svc** project. 
* Click **Show In** 
* Click **System Explorer**.
![Deploy ImageManager Lambda Function](/images/5-use-codestar-orchestration/5.2-redeploy-through-cicd-pipeline/redeploy-through-cicd-pipeline-002.png?featherlight=false&width=90pc)
4. Delete the contents of the **/src** folder and the **/target** folder of the **dev-flight-svc** project before copying.
* Copy the contents of the **FlightSpecials** project to the folder we opened in the step 3
{{% notice warning %}} 
If you don't delete the contents of the **/src** folder and the **/target** folder before copying, the process of building will be error because we don't configure **HelloWorldController / Handler**.
{{% /notice %}}
5. In the Eclipse IDE, right-click on the **dev-flight-svc** project.
* Click **Maven**
* Click **Update Project...**
![Deploy ImageManager Lambda Function](/images/5-use-codestar-orchestration/5.2-redeploy-through-cicd-pipeline/redeploy-through-cicd-pipeline-003.png?featherlight=false&width=90pc)
6. Click **OK**
![Deploy ImageManager Lambda Function](/images/5-use-codestar-orchestration/5.2-redeploy-through-cicd-pipeline/redeploy-through-cicd-pipeline-004.png?featherlight=false&width=90pc)

{{% notice note %}} 
As part of our microservice setup, we will be using VPC integration and assigning a new IAM Role to our Lambda function to allow it to perform various tasks. When CodeStar created our project, it created an IAM Role that gave CloudFormation just enough permissions to deploy the Hello World example service. These permissions are not enough for our more advanced requirements. So we need to adjust the policies assigned to the CloudFormation role, to extend those permissions.
{{% /notice %}}

7. Go to [**AWS IAM Console**](https://console.aws.amazon.com/iamv2/).
* Click **Roles**.
* Type ```CodeStarWorker-dev-flight-svc-CloudFormation``` to the search bar 
* Click **CodeStarWorker-dev-flight-svc-CloudFormation**
![Deploy ImageManager Lambda Function](/images/5-use-codestar-orchestration/5.2-redeploy-through-cicd-pipeline/redeploy-through-cicd-pipeline-005.png?featherlight=false&width=90pc)
8. Click **Add permissions**
* Click **Create inline policy**
![Deploy ImageManager Lambda Function](/images/5-use-codestar-orchestration/5.2-redeploy-through-cicd-pipeline/redeploy-through-cicd-pipeline-006.png?featherlight=false&width=90pc)
9. Click tab **JSON**
* Paste the following content
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
10. In the **Name** section, type ```idevelopCodeStarCloudFormationPolicy```
* Click **Create policy**
![Deploy ImageManager Lambda Function](/images/5-use-codestar-orchestration/5.2-redeploy-through-cicd-pipeline/redeploy-through-cicd-pipeline-008.png?featherlight=false&width=90pc)

{{% notice info %}} 
When attached to the **CodeStarWorker-dev-flight-svc-CloudFormation** role, the permissions in the **idevelopCodeStarCloudFormationPolicy** policy allow CloudFormation to act on your behalf when deploying the changes to your environment, and include authority to allow the Lambda function to attach to the VPC where the RDS instance that hosts our TravelBuddy website is deployed. They also allow CloudFormation to create a new IAM Role that the Lambda function will use to execute.
{{% /notice %}}

11. The CloudFormation template **template.yml** that was provided as part of the **FlightSpecials.zip** file has some placeholder values that you need to update to match the values from your lab account before you can deploy the updates. These include Subnet Ids, Security Group Ids and the RDS Instance Endpoint, which are unique to your lab account and unknown at this stage to the template.

* Go to [**Amazon EC2 console**](https://console.aws.amazon.com/ec2/).
* Click **Security Groups**.
* Type ```DBSecurityGroup``` to the search bar and press **Enter**
* Save the **Security Group Id** value of the **DBSecurityGroup** Security Group
![Deploy ImageManager Lambda Function](/images/5-use-codestar-orchestration/5.2-redeploy-through-cicd-pipeline/redeploy-through-cicd-pipeline-009.png?featherlight=false&width=90pc)
12. Go to [**Amazon VPC console**](https://console.aws.amazon.com/vpc/).
* Click **Subnets**.
* Type ```Module3/DevAxNetworkVPC/privateSubnet``` to the search bar and press **Enter**
* Save the **Subnet ID** value of the **Module3/DevAxNetworkVPC/privateSubnet1** subnet and the **Subnet ID** value of the **Module3/DevAxNetworkVPC/privateSubnet2** subnet
![Deploy ImageManager Lambda Function](/images/5-use-codestar-orchestration/5.2-redeploy-through-cicd-pipeline/redeploy-through-cicd-pipeline-010.png?featherlight=false&width=90pc)
13. Go to [AWS CloudFormation Console](https://console.aws.amazon.com/cloudformation/).
* Type **aws-stack-for-Devax-lab03** to the search bar and press **Enter**.
* Click **aws-stack-for-Devax-lab03**.
![Deploy ImageManager Lambda Function](/images/5-use-codestar-orchestration/5.2-redeploy-through-cicd-pipeline/redeploy-through-cicd-pipeline-011.png?featherlight=false&width=90pc)
14. Click **Output**
* Save the **RDSEndpoint** value
![Deploy ImageManager Lambda Function](/images/5-use-codestar-orchestration/5.2-redeploy-through-cicd-pipeline/redeploy-through-cicd-pipeline-012.png?featherlight=false&width=90pc)
15. In the Eclipse IDE, open the **template.yml** the
* Replace **sg-\<REPLACE\>** the **Security Group Id** value of the **DBSecurityGroup** Security Group we saved in the step 11
* Replace **subnet-\<REPLACE\>** with the **Subnet ID** value of the **Module3/DevAxNetworkVPC/privateSubnet1** subnet and the **Subnet ID** value of the **Module3/DevAxNetworkVPC/privateSubnet2** with subnet we saved in the step 12
* Replace **\<RDSEndpoint\>** with the **RDSEndpoint** value we saved in the step 14
* Save
![Deploy ImageManager Lambda Function](/images/5-use-codestar-orchestration/5.2-redeploy-through-cicd-pipeline/redeploy-through-cicd-pipeline-013.png?featherlight=false&width=90pc)