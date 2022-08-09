+++
title = "Upload and test Lambda function on AWS Lambda"
date = 2020
weight = 2
chapter = false
pre = "<b>3.2. </b>"
+++
#### Upload and test Lambda function on AWS Lambda

1. We will now deploy the Lambda function to your account so you can test it. Right-click on the **LambdaFunctionHandler.java** class in the **src/main/java/idevelop.lambda.s3handler** element
* Click **AWS Lambda**
* Click **Run function on AWS Lambda…**
![Upload and test Lambda function on AWS Lambda](/images/3-create-serverless-microservices/3.2-update-and-test/update-and-test-001.png?featherlight=false&width=90pc)
2. Click **Upload now**
![Upload and test Lambda function on AWS Lambda](/images/3-create-serverless-microservices/3.2-update-and-test/update-and-test-002.png?featherlight=false&width=90pc)
3. Select your **Region**
* Select **Create a new Lambda function**.
* The name of the Lambda function is ```TestLambda```
* Click **Next**
![Upload and test Lambda function on AWS Lambda](/images/3-create-serverless-microservices/3.2-update-and-test/update-and-test-003.png?featherlight=false&width=90pc)
4. In the **description** section, type ```Test AWS Lambda function triggered by S3 upload```
* In the **IAM role** section, select **LambdaRole** which has been created automatically by the lab setup.
* In the **S3 bucket** section, select the S3 bucket whose name starts with **idevelop-sourcecode-** we created in the prerequisites section to host our Lambda source code
* Select **Finish** to upload the function into your AWS Account. 
![Upload and test Lambda function on AWS Lambda](/images/3-create-serverless-microservices/3.2-update-and-test/update-and-test-004.png?featherlight=false&width=90pc)
5. It will take a few moments to upload. 
* Go to [**AWS Lambda Console**](https://console.aws.amazon.com/lambda) 
* Click **Functions**. 
* Type ```TestLambda``` to the search bar and press **Enter**. We will see the **TestLambda** function is uploaded.
* Click **TestLambda**
![Upload and test Lambda function on AWS Lambda](/images/3-create-serverless-microservices/3.2-update-and-test/update-and-test-005.png?featherlight=false&width=90pc)
6. Click **Add Triggers** 
![Upload and test Lambda function on AWS Lambda](/images/3-create-serverless-microservices/3.2-update-and-test/update-and-test-006.png?featherlight=false&width=90pc)
7. In the **Trigger configuration** section, select **S3**
* In the **Bucket** section, select the S3 bucket whose name starts with **idevelop-imagemanager-** we created for uploads the images previously
* In the **Event type** section, select **All Object create events**
* In the **Prefix** section, type ```uploads/```.
* Click **I acknowledge that using the same S3 bucket for both input and output is not recommended and that this configuration can cause recursive invocations, increased Lambda usage, and increased costs.**
* Click **Add**
![Upload and test Lambda function on AWS Lambda](/images/3-create-serverless-microservices/3.2-update-and-test/update-and-test-007.png?featherlight=false&width=90pc)
8. We will now test the function. Go to [**AWS S3 Console**](https://s3.console.aws.amazon.com/s3/)
* Type ```idevelop-imagemanager``` to the search bar
* Click the S3 bucket whose name starts with **idevelop-imagemanager**
![Upload and test Lambda function on AWS Lambda](/images/3-create-serverless-microservices/3.2-update-and-test/update-and-test-008.png?featherlight=false&width=90pc)
9. Click **Create folder**
![Upload and test Lambda function on AWS Lambda](/images/3-create-serverless-microservices/3.2-update-and-test/update-and-test-009.png?featherlight=false&width=90pc)
10. In the **Create folder** page
* In the **Folder name** section, type ```uploads```
* Click **Create folder**
![Upload and test Lambda function on AWS Lambda](/images/3-create-serverless-microservices/3.2-update-and-test/update-and-test-010.png?featherlight=false&width=90pc)
11. Click **uploads/** to open the **uploads** folder
![Upload and test Lambda function on AWS Lambda](/images/3-create-serverless-microservices/3.2-update-and-test/update-and-test-011.png?featherlight=false&width=90pc)

{{%attachments title="Puppy image" style="orange" pattern="Puppy.jpg"/%}}

12. Download the **Puppy.jpg** file

13. Click **Upload**
![Upload and test Lambda function on AWS Lambda](/images/3-create-serverless-microservices/3.2-update-and-test/update-and-test-012.png?featherlight=false&width=90pc)
14. Click **Add files**
* Select the **Puppy.jpg** file we downloaded in the step 12
* Click **Upload**
![Upload and test Lambda function on AWS Lambda](/images/3-create-serverless-microservices/3.2-update-and-test/update-and-test-013.png?featherlight=false&width=90pc)
15. Once the upload is complete
* Go to [**AWS Lambda Console**](https://console.aws.amazon.com/lambda)
* Click **Funtions**
* Type ```TestLambda``` to the search bar and press **Enter**
* Click **TestLambda**
![Upload and test Lambda function on AWS Lambda](/images/3-create-serverless-microservices/3.2-update-and-test/update-and-test-014.png?featherlight=false&width=90pc)
16. Click the **Monitor** tab
* You will see 2 invocation counts and their associated invocation durations in the graphs. You see two because one is the creation of the uploads/ folder, and the other is the upload of the Puppy.jpg image.
![Upload and test Lambda function on AWS Lambda](/images/3-create-serverless-microservices/3.2-update-and-test/update-and-test-015.png?featherlight=false&width=90pc)
17. Click **View logs in CloudWatch** to open the CloudWatch logs for this Lambda function. 
![Upload and test Lambda function on AWS Lambda](/images/3-create-serverless-microservices/3.2-update-and-test/update-and-test-016.png?featherlight=false&width=90pc)
* We will see the logged **log streams**
![Upload and test Lambda function on AWS Lambda](/images/3-create-serverless-microservices/3.2-update-and-test/update-and-test-017.png?featherlight=false&width=90pc)
* Click the first log steam to see the detail information
![Upload and test Lambda function on AWS Lambda](/images/3-create-serverless-microservices/3.2-update-and-test/update-and-test-018.png?featherlight=false&width=90pc)
{{% notice note %}} 
Notice that you will see the CONTENT TYPE output, as per the code that we have uploaded. In the first event, the CONTENT TYPE is application/x-directory and the second mention of CONTENT TYPE notes the type as image/jpeg. The sample lambda function code provided by the New Lambda Function wizard simply logs the CONTENT TYPE of the file, with no other processing. We will change this behaviour shortly.
{{% /notice %}}
18. Upload an any file is not a image (do the same the step 13 and 14).
![Upload and test Lambda function on AWS Lambda](/images/3-create-serverless-microservices/3.2-update-and-test/update-and-test-019.png?featherlight=false&width=90pc)
* Inspect the logs (do the same the step 15, 16 and 17)
![Upload and test Lambda function on AWS Lambda](/images/3-create-serverless-microservices/3.2-update-and-test/update-and-test-020.png?featherlight=false&width=90pc)