+++
title = "Introduction"
date = 2020
weight = 1
chapter = false
pre = "<b>1. </b>"
+++
#### Introduction

In this lab, you will learn how to create an AWS Lambda function that is triggered when files are uploaded to an Amazon S3 bucket. You will upload your code for the Lambda function, and then manually wire-up an S3 event trigger to invoke the function and view the output logs. Then you will iterate on the code to add functionality to allow it to process files of different types - specifically, to create thumbnails for JPG images, and delete all other file types. Finally, you will create a deployment package and automate the deployment of the function and associated triggers and S3 buckets, using the AWS CLI.

![Architecture](/images/1-introduction/info.png?featherlight=false&width=90pc)

The final version will perform the following tasks:
* First, a file is uploaded to an S3 bucket
* Amazon S3 triggers the AWS Lambda function associated with the PutObject action and provides metadata to describe the file
* The Lambda function inspects the content type of the file and if it is not an **image/jpeg** file, the file is deleted. If the file is an **image/jpeg** file, the code will generate a thumbnail of the image and store the thumbnail in a different folder in the same bucket.

Later in the lab, you will return to the TravelBuddy web application, and deploy parts of the monolithic codebase as a standalone microservices, making use of AWS CodeStar to create the CI/CD pipeline.

![Architecture](/images/1-introduction/monolithcallingmicro.png?featherlight=false&width=90pc)

#### Topics Covered
By the end of this lab, you will be able to:
* Use the Eclipse IDE to create and deploy an AWS Lambda function.
* Edit the supplied generic Java Lambda function source code to resize images as thumbnails or delete non-image files, and wire-up a **trigger** to an S3 bucket to test the feature.
* Use **AWS Serverless Application Model(SAM)** and **AWS CloudFormation** to create a template to automate the deployment of your Lambda function an S3 trigger and an S3 bucket.
* Identify a microservice candidate in a monolithic codebase, and create a AWS CodeStar project to manage a CI/CD pipeline for that microservice hosted in AWS Lambda.

#### Technical Knowledge Prerequisites
To successfully complete this lab, you should be familiar with basic navigation of the AWS Management Console and be comfortable editing scripts using a text editor.

#### Environment
The following diagram depicts the resources that were deployed in your AWS account.
![Architecture](/images/1-introduction/architecture.png?featherlight=false&width=50pc)