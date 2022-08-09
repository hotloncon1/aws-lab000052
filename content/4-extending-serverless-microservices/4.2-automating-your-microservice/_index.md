+++
title = "Automating Your Serverless Microservice"
date = 2020
weight = 2
chapter = false
pre = "<b>4.2. </b>"
+++
#### Automating Your Serverless Microservice


In the previous exercise, you used the Eclipse IDE to create using the AWS Toolkit for Eclipse and update a Lambda function. This allowed you to manually initiate automatic upload of your Lambda function. However, this mechanism may not be convenient for automating deployment steps for functions, or coordinating deployments and updates to other elements of a serverless application, such as event sources and downstream resources. For example, the Eclipse IDE does not give you the ability to deploy and update the S3 bucket and wire up the S3 PUT OBJECT trigger, together with your Lambda function as one deployment unit.

You can use **AWS CloudFormation** to easily specify, deploy, and configure serverless applications. AWS CloudFormation is a service that helps you model and set up your Amazon Web Services resources so that you can spend less time managing those resources and more time focusing on your applications that run in AWS. You create a template that describes all the AWS resources that you want (like Lambda functions and S3 buckets), and AWS CloudFormation takes care of provisioning and configuring those resources for you.

In addition, you can use the **AWS Serverless Application Model (SAM)** to express resources that comprise the serverless application. These resource types, such as AWS Lambda functions and APIs, are fully supported by AWS CloudFormation and make it easier for you to define and deploy your serverless application.

In this exercise, you will use **AWS CLI** and **AWS CloudFormation/SAM **to package up your solution and deploy it from scratch, without having to manually create or configure any dependencies.

1. In the Eclipse IDE, right-click on the **TestLambda** project
* Click **New**
* Click **File**
![Deploy ImageManager Lambda Function](/images/4-extending-serverless-microservices/4.2-automating-your-microservice/automating-your-microservice-001.png?featherlight=false&width=90pc)
2. In the **File name** section, điền ```template.yaml```
* Click **Finish**
![Deploy ImageManager Lambda Function](/images/4-extending-serverless-microservices/4.2-automating-your-microservice/automating-your-microservice-002.png?featherlight=false&width=90pc)
3. The **template.yaml** file in the same folder as the **pom.xml** file of the **TestLambda** project
* Repalce the content of the **template.yaml** file with the following content
```
AWSTemplateFormatVersion: '2010-09-09'
Transform: 'AWS::Serverless-2016-10-31'
Description: Testing lambda and S3
Resources:
  TestLambda:
    Type: 'AWS::Serverless::Function'
    Properties:
      Handler: idevelop.lambda.s3handler.LambdaFunctionHandler
      Runtime: java8
      CodeUri: target/s3handler-1.0.0.jar
      Description: Testing lambda and S3
      MemorySize: 512
      Timeout: 15
      Role: !Sub arn:aws:iam::${AWS::AccountId}:role/LambdaRole
      Events:
        CreateThumbnailEvent:
          Type: S3
          Properties:
            Bucket:
              Ref: ImageManagerSrcBucket
            Events:
              - 's3:ObjectCreated:Put'
            Filter:
              S3Key:
                Rules:
                  - Name: prefix
                    Value: uploads/

  ImageManagerSrcBucket:
    Type: AWS::S3::Bucket
    Properties:
      BucketName: !Sub idevelop-imagemanager-${AWS::AccountId}
```
* Save
![Deploy ImageManager Lambda Function](/images/4-extending-serverless-microservices/4.2-automating-your-microservice/automating-your-microservice-003.png?featherlight=false&width=90pc)
4. We will use the AWS CLI to push the **template.yaml** file to an S3 bucket where it can then be deployed
* In the Command prompt, Execute the following command
```
aws cloudformation package --template-file template.yaml --s3-bucket <YOUR_CODE_BUCKET_NAME> --output-template deploy-template.yaml --profile devaxacademy
```
{{% notice note %}} 
Repale **<YOUR_CODE_BUCKET_NAME>** with the name of the **S3 bucket** whose name starts with **idevelop-sourcecode** we created
{{% /notice %}}
![Deploy ImageManager Lambda Function](/images/4-extending-serverless-microservices/4.2-automating-your-microservice/automating-your-microservice-004.png?featherlight=false&width=60pc)
{{% notice note %}} 
If you see an error message similar to **‘NoneType’ object has no attribute ‘items’** check the formatting of your **template.yaml** file. This error indicates a problem with the indentation!
{{% /notice %}}

{{% notice info %}} 
The **aws cloudformation package** command takes the AWS SAM template provided and re-writes it in terms of the artefacts automatically uploaded to the specified S3 bucket. In this case **deploy-template.yaml** is created, and contains the **CodeUri** value that points to the deployment zip in the Amazon S3 bucket that you specified. This template represents your serverless application.
{{% /notice %}}
5. You are now ready to deploy the JAR artifact as a Lambda function, and wire-up the S3 trigger to a new S3 bucket. You will notice the last output from the previous command instructs us what to run in order to deploy the packaged template
* In the Command prompt, Execute the following command
```
aws cloudformation deploy --template-file deploy-template.yaml --stack-name ImageManagerDemo --profile devaxacademy
```
![Deploy ImageManager Lambda Function](/images/4-extending-serverless-microservices/4.2-automating-your-microservice/automating-your-microservice-005.png?featherlight=false&width=60pc)
6. Go to [**Amazon CloudFormation Console**](https://console.aws.amazon.com/cloudformation/home).
* Click **Stacks**
* Type ```ImageManagerDemo``` to the search bar and press **Enter**
* We will see the CloudFormation stack whose name is **ImageManagerDemo** was created
![Deploy ImageManager Lambda Function](/images/4-extending-serverless-microservices/4.2-automating-your-microservice/automating-your-microservice-006.png?featherlight=false&width=90pc)
{{% notice info %}} 
You can watch the progress of the creation of the resources directly in the CloudFormation console. \
The CloudFormation template creates a new S3 Bucket called **idevelop-imagemanager-<YOUR_ACCOUNT_ID>** where **<YOUR_ACCOUNT_ID>** is **AWS account ID** assigned to your lab environment.\
The template also creates a new Lambda function called **ImageManagerDemo-TestLambda-XXXXXX** where **XXXXXX** is a random identifier allocated by CloudFormation to ensure uniqueness of the function name.
{{% /notice %}}
7. Go to [**AWS S3 Console**](https://s3.console.aws.amazon.com/s3/)
* Type ```idevelop-imagemanager``` to the search bar
* Click S3 bucket **idevelop-imagemanager-<YOUR_ACCOUNT_ID>**
![Deploy ImageManager Lambda Function](/images/4-extending-serverless-microservices/4.2-automating-your-microservice/automating-your-microservice-007.png?featherlight=false&width=90pc)
8. Click **Create folder**
![Deploy ImageManager Lambda Function](/images/4-extending-serverless-microservices/4.2-automating-your-microservice/automating-your-microservice-008.png?featherlight=false&width=90pc)
9. In the **Folder name** section, type ```uploads```
* Click **Create folder**
![Deploy ImageManager Lambda Function](/images/4-extending-serverless-microservices/4.2-automating-your-microservice/automating-your-microservice-009.png?featherlight=false&width=90pc)
10. Click the **uploads/** folder
![Deploy ImageManager Lambda Function](/images/4-extending-serverless-microservices/4.2-automating-your-microservice/automating-your-microservice-010.png?featherlight=false&width=90pc)
11. Click **Upload**
![Deploy ImageManager Lambda Function](/images/4-extending-serverless-microservices/4.2-automating-your-microservice/automating-your-microservice-011.png?featherlight=false&width=90pc)
12. Click **Add files**
* Select the **Puppy.jpg** file we downloaded
* CLick **Add files**
* Select the non-image file
* Click **Upload**
![Deploy ImageManager Lambda Function](/images/4-extending-serverless-microservices/4.2-automating-your-microservice/automating-your-microservice-012.png?featherlight=false&width=90pc)
13. Click **Close**
![Deploy ImageManager Lambda Function](/images/4-extending-serverless-microservices/4.2-automating-your-microservice/automating-your-microservice-013.png?featherlight=false&width=90pc)
14. We will see that non-image file is automatically deleted
* Click **idevelop-imagemanager-<YOUR_ACCOUNT_ID>**
![Deploy ImageManager Lambda Function](/images/4-extending-serverless-microservices/4.2-automating-your-microservice/automating-your-microservice-014.png?featherlight=false&width=90pc)
15. Click the **processed** folder
![Deploy ImageManager Lambda Function](/images/4-extending-serverless-microservices/4.2-automating-your-microservice/automating-your-microservice-015.png?featherlight=false&width=90pc)
16. We will see a thumbnail of the **Puppy.jpg** file
![Deploy ImageManager Lambda Function](/images/4-extending-serverless-microservices/4.2-automating-your-microservice/automating-your-microservice-016.png?featherlight=false&width=90pc)
17. Go to [**Amazon CloudFormation Console**](https://console.aws.amazon.com/cloudformation/home).
* Click **Stacks**
* Type ```ImageManagerDemo``` to the search bar **Enter**
* Click the name of the CloudFormation stack
![Deploy ImageManager Lambda Function](/images/4-extending-serverless-microservices/4.2-automating-your-microservice/automating-your-microservice-017.png?featherlight=false&width=90pc)
18. Click **Monitor**
* Click **View logs in CloudWatch**
![Deploy ImageManager Lambda Function](/images/4-extending-serverless-microservices/4.2-automating-your-microservice/automating-your-microservice-018.png?featherlight=false&width=90pc)
19. Click the first **Log stream** in the **Log streams** table
![Deploy ImageManager Lambda Function](/images/4-extending-serverless-microservices/4.2-automating-your-microservice/automating-your-microservice-019.png?featherlight=false&width=90pc)
20. We see the process in the **CloudWatch log**
![Deploy ImageManager Lambda Function](/images/4-extending-serverless-microservices/4.2-automating-your-microservice/automating-your-microservice-020.png?featherlight=false&width=90pc)