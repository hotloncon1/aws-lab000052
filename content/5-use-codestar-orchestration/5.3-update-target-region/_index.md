+++
title = "Update target AWS region for API"
date = 2020
weight = 3
chapter = false
pre = "<b>5.3. </b>"
+++
#### Update target AWS region for API

The **swagger.yml** file provided in the zip bundle is the definition for the API that exposes the microservice via Amazon API Gateway. It needs to be updated with details of your lab AWS Account Id and target AWS Region before you can deploy your microservice.
1. In the Eclipse IDE, open the **swagger.yml** file
* Press **Ctrl+F** shortcut
* In the **Find** section, type ```<REGION>```
* In the **Replace with** section, type your **Region** 
* Click **Replace All** to replace
![Deploy ImageManager Lambda Function](/images/5-use-codestar-orchestration/5.3-update-target-region/update-target-region-001.png?featherlight=false&width=90pc)
2. Do the same as the step 1 to relace ```<ACCOUNTID>``` with your **AWS Account Id**
* Save
3. In the Command Prompt, execute the following command to configure your git client
```
C:\Users\Administrator\git\dev-flight-svc>git config --global user.name "awsstudent"
C:\Users\Administrator\git\dev-flight-svc>git config --global user.email "<YOUR_EMAIL>"
```
{{% notice note %}} 
Replace **<YOUR_EMAIL>** with your email
{{% /notice %}}
![Deploy ImageManager Lambda Function](/images/5-use-codestar-orchestration/5.3-update-target-region/update-target-region-002.png?featherlight=false&width=60pc)
4. Execute the following command to review the changed code files
```
git status
```
![Deploy ImageManager Lambda Function](/images/5-use-codestar-orchestration/5.3-update-target-region/update-target-region-003.png?featherlight=false&width=60pc)
5. Execute the below command to add in the changed files
```
git add .
git commit -m "Baseline implementation"
```
![Deploy ImageManager Lambda Function](/images/5-use-codestar-orchestration/5.3-update-target-region/update-target-region-004.png?featherlight=false&width=60pc)
6. Execute the below command to switch back to the **master** branch
```
git checkout master
```
![Deploy ImageManager Lambda Function](/images/5-use-codestar-orchestration/5.3-update-target-region/update-target-region-005.png?featherlight=false&width=60pc)
7. Execute the below command to merge the changes for your **new-implementation** branch into the **master** branch
```
git merge new-implementation
```
![Deploy ImageManager Lambda Function](/images/5-use-codestar-orchestration/5.3-update-target-region/update-target-region-006.png?featherlight=false&width=60pc)
8. In the Eclipse IDE, right-click on the **dev-flight-svc** project
* Click **Team** 
* Click **Push to origin**. 
![Deploy ImageManager Lambda Function](/images/5-use-codestar-orchestration/5.3-update-target-region/update-target-region-007.png?featherlight=false&width=90pc)
9. Click **Close**
![Deploy ImageManager Lambda Function](/images/5-use-codestar-orchestration/5.3-update-target-region/update-target-region-008.png?featherlight=false&width=90pc)
10. Go to [**Amazon EC2 console**](https://console.aws.amazon.com/ec2/).
* Click **Security Groups**.
* Type ```DBSecurityGroup``` to the search bar **Enter**
* Select **DBSecurityGroup**
* Click tab **Inbound rules**
* Click **Edit inbound rules**
![Deploy ImageManager Lambda Function](/images/5-use-codestar-orchestration/5.3-update-target-region/update-target-region-009.png?featherlight=false&width=90pc)
11. In the **Source** section, select **DBSecurityGroup**
* Click **Save rules**
![Deploy ImageManager Lambda Function](/images/5-use-codestar-orchestration/5.3-update-target-region/update-target-region-010.png?featherlight=false&width=90pc)
12. Go to [**AWS CodeStar Console**](https://console.aws.amazon.com/codesuite/codestar/home).
* Click **Projects**
* Click **dev-flight-svc**
![Deploy ImageManager Lambda Function](/images/5-use-codestar-orchestration/5.3-update-target-region/update-target-region-011.png?featherlight=false&width=90pc)
13. Click **View application**
![Deploy ImageManager Lambda Function](/images/5-use-codestar-orchestration/5.3-update-target-region/update-target-region-012.png?featherlight=false&width=90pc)
14. When the page opens, you should see an error **{"message":"Missing Authentication Token"}**. This is to be expected, since you are attempting to hit the root of the API, rather than a specific microservice. Edit the URL in the browser to add **flightspecials** to the end of the URL
![Deploy ImageManager Lambda Function](/images/5-use-codestar-orchestration/5.3-update-target-region/update-target-region-013.png?featherlight=false&width=90pc)
15. We will see the result like:
![Deploy ImageManager Lambda Function](/images/5-use-codestar-orchestration/5.3-update-target-region/update-target-region-014.png?featherlight=false&width=90pc)