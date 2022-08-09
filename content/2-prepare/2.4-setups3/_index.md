+++
title = "Setup S3 buckets"
date = 2020
weight = 4
chapter = false
pre = "<b>2.4. </b>"
+++
#### Setup S3 buckets 
You need to setup two buckets. One bucket will contain uploaded lambda functions, the other bucket will be used to store images.
{{% notice note %}} 
You can create S3 bucket from the AWS Console, or from the command line
{{% /notice %}}
1. Open Command Prompt
2. Execute the following command:
```
aws s3 mb s3://idevelop-sourcecode-<yourinitials> --region <region> --profile devaxacademy
aws s3 mb s3://idevelop-imagemanager-<yourinitials> --region <region> --profile devaxacademy
```
{{% notice note %}} 
Replace ```<yourinitials>``` with your name to create unique name for your bucket.\
Replace ```<region>``` with your **region**.
{{% /notice %}}
![Setup S3 bucket](/images/2-prepare/2.4-setups3/setups3-001.png?featherlight=false&width=60pc)
3. Go to [**AWS S3 Console**](https://s3.console.aws.amazon.com/s3/).
* Type ```idevelop``` to the search bar, we will see two buckets
![Setup S3 bucket](/images/2-prepare/2.4-setups3/setups3-002.png?featherlight=false&width=90pc)