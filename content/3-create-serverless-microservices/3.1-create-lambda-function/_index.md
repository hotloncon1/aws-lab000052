+++
title = "Create and test Lambda function locally"
date = 2020
weight = 1
chapter = false
pre = "<b>3.1. </b>"
+++
#### Create and test Lambda function locally

1. Open **Eclipse IDE** in the Windows virtual machine.
* select the **AWS toolkit icon** to expand the menu
* Click **New AWS Lambda Java Project…**
![Create and test Lambda function locally](/images/3-create-serverless-microservices/3.1-create-lambda-function/create-lambda-function-001.png?featherlight=false&width=90pc)
2. In the **New AWS Lambda Maven Project** dialog
* In the **Project name** section, type ```TestLambda```
* In the **Group ID** section, type ```idevelop.lambda```
* In the **Artifact ID** section, type ```s3handler```
* Click **Finish**
![Create and test Lambda function locally](/images/3-create-serverless-microservices/3.1-create-lambda-function/create-lambda-function-002.png?featherlight=false&width=90pc)
3. We need to update the pom.xml file that Maven uses to newer version of Mockito for our tests
* Open the **TestLambda** project
* Open the **pom.xml** file
* Find the **dependency** section whose **artifactId** is **mockito-core** and replace the **version** value with ```3.3.3```
* Save
![Create and test Lambda function locally](/images/3-create-serverless-microservices/3.1-create-lambda-function/create-lambda-function-003.png?featherlight=false&width=90pc)
4. Run the **JUnit Test**
* Right-click on the **TestLambda** project. Click **Run As**
* Click **JUnit Test**
![Create and test Lambda function locally](/images/3-create-serverless-microservices/3.1-create-lambda-function/create-lambda-function-004.png?featherlight=false&width=90pc)
5. You will see outputs from the lambda function, as if it were triggered by a file uploaded to S3. The parameters for the test are provided in the test resources, in the form of a JSON payload which resembles the payload the Amazon environment will send to the Lambda function, when the S3 bucket associated with this Lambda function receives an uploaded file. 
![Create and test Lambda function locally](/images/3-create-serverless-microservices/3.1-create-lambda-function/create-lambda-function-005.png?featherlight=false&width=90pc)
{{% notice note %}} 
You may see some WARNINGS regarding profile name. You can safely ignore those warnings for this lab.
{{% /notice %}}
6. To see the output of the tests, click on the **JUnit** tab
![Create and test Lambda function locally](/images/3-create-serverless-microservices/3.1-create-lambda-function/create-lambda-function-006.png?featherlight=false&width=90pc)
7. Examine the S3-event.put.json file. The S3-event.put.json file contains the required schema and values that we will use for this lab.
![Create and test Lambda function locally](/images/3-create-serverless-microservices/3.1-create-lambda-function/create-lambda-function-007.png?featherlight=false&width=90pc)
{{% notice note %}} 
You may see a warning regarding node being missing. You can safely ignore those warnings for this lab.
{{% /notice %}}

#### Update the provided code to handle URL encoded keys
The source code provided by the tooling does not take into consideration the encoding applied to the key name provided in the S3 event when it is sent to your Lambda function, and so, if you upload a file for testing, and the file contains spaces or punctuation (for example) then the string needs to be decoded before you use it.

8. Open the **LambdaFunctionHandler.java** file whose path is **src/main/java/idevelop.lambda.s3handler/LambdaFunctionHandler.java**
* Add this code after line 28 and save
```
try
{
  key = java.net.URLDecoder.decode(key, "UTF-8");
}
catch(Exception ex)
{
  context.getLogger().log("Could not decode URL for keyname... continuing...");
}
```
![Create and test Lambda function locally](/images/3-create-serverless-microservices/3.1-create-lambda-function/create-lambda-function-008.png?featherlight=false&width=90pc)