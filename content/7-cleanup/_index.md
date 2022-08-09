+++
title = "Clean up resources"
date = 2020
weight = 7
chapter = false
pre = "<b>7. </b>"
+++
You clean up resources in the following order:

#### Terminate EC2 Instance
1. Go to [**Amazon EC2 console**](https://console.aws.amazon.com/ec2/).
* On the left navigation bar, click **Intances**.
* Select **DevAxWindowsHost**. 
* Click **Instance state**
* Click **Terminate instance**
![Clean up reources](/images/7-cleanup/cleanup-001.png?featherlight=false&width=90pc)
2. Click **Terminate**
![Clean up reources](/images/7-cleanup/cleanup-002.png?featherlight=false&width=90pc)

#### Delete Users
1. Go to [**AWS IAM Console**](https://console.aws.amazon.com/iamv2/).
* Click **Users**.
* Type ```awsstudent``` to the search bar and press **Enter**
* Select **awsstudent**. 
* Click **Delete**
![Clean up reources](/images/7-cleanup/cleanup-003.png?featherlight=false&width=90pc)
2. Type ```awsstudent``` to confirm, then click **Delete**
![Clean up reources](/images/7-cleanup/cleanup-004.png?featherlight=false&width=90pc)

#### Delete CodeStar
1. Go to [**AWS CodeStar Console**](https://console.aws.amazon.com/codesuite/codestar/home).
* Click **Projects**
* Select **dev-flight-svc**
* Click **Delete**
![Clean up reources](/images/7-cleanup/cleanup-005.png?featherlight=false&width=90pc)
2. Type ```delete``` to confirm, then click **Delete** to delete
![Clean up reources](/images/7-cleanup/cleanup-006.png?featherlight=false&width=90pc)

#### Delete S3 bucket
1. Go to [**AWS S3 Console**](https://s3.console.aws.amazon.com/s3/).
* Click **Buckets**
* Select the S3 bucket whose name starts with **idevelop-sourcecode-**.
* Click **Empty**.
![Clean up reources](/images/7-cleanup/cleanup-007.png?featherlight=false&width=90pc)
2. Type ```permanently delete``` to confirm, then click **Empty** to delete the data of the S3 bucket.
![Clean up reources](/images/7-cleanup/cleanup-008.png?featherlight=false&width=90pc)
3. Click **Exit** to back S3 interface.
![Clean up reources](/images/7-cleanup/cleanup-009.png?featherlight=false&width=90pc)
4. Click **Delete**.
![Clean up reources](/images/7-cleanup/cleanup-010.png?featherlight=false&width=90pc))
5. Type the name of the bucket then click **Delete bucket** to delete S3 bucket.
![Clean up reources](/images/7-cleanup/cleanup-011.png?featherlight=false&width=90pc)
6. Do the same for the other S3 bucket

#### Delete CloudFormation Stack
1. Go to [AWS CloudFormation Console](https://console.aws.amazon.com/cloudformation/).
* Select **aws-stack-for-Devax-lab03**.
* Click **Delete**
![Clean up reources](/images/7-cleanup/cleanup-012.png?featherlight=false&width=90pc)
2. Click **Delete stack**
![Clean up reources](/images/7-cleanup/cleanup-013.png?featherlight=false&width=90pc)

#### Delete Lambda function
1. Go to [**AWS Lambda console**](https://console.aws.amazon.com/lambda/home).
* Click **Functions**.
* Select **TestLambda**.
* Click **Actions**
* Click **Delete**
![Clean up reources](/images/7-cleanup/cleanup-014.png?featherlight=false&width=90pc)
2. Type ```delete``` to confirm, then click **Delete** to delete
![Clean up reources](/images/7-cleanup/cleanup-015.png?featherlight=false&width=90pc)

#### Delete RDS Snapshot
1. Go to [**AWS RDS console**](https://console.aws.amazon.com/rds/home).
* Click **Snapshots**
* Select **RDS Snapshot** was created in this lab
* Click **Actions**
* Click **Delete snapshot**
![Clean up reources](/images/7-cleanup/cleanup-016.png?featherlight=false&width=90pc)
2. Click **Delete** to delete
![Clean up reources](/images/7-cleanup/cleanup-017.png?featherlight=false&width=90pc)