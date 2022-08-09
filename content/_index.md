+++
title = "Configuring Application Release Automation"
date = 2021
weight = 1
chapter = false
+++
# Configuring Application Release Automation

#### Oveview

In this lab, you will learn how to create an AWS Lambda function that is triggered when files are uploaded to an Amazon S3 bucket. You will upload your code for the Lambda function, and then manually wire-up an S3 event trigger to invoke the function and view the output logs. Then you will iterate on the code to add functionality to allow it to process files of different types - specifically, to create thumbnails for JPG images, and delete all other file types. Finally, you will create a deployment package and automate the deployment of the function and associated triggers and S3 buckets, using the AWS CLI.

![Architecture](/images/1-introduction/info.png?featherlight=false&width=90pc)

#### Content:

1. [Introduction](1-introduction/)
2. [Preparation](2-prepare/)
3. [Create a Serverless Microservice](3-create-serverless-microservices/)
4. [Extending Serverless Microservices](4-extending-serverless-microservices/)
5. [Configure orchestration with CodeStar](5-use-codestar-orchestration/)
6. [Challenge - Expose the microservice API](6-challenge/)
7. [Clean up resources](7-cleanup/)