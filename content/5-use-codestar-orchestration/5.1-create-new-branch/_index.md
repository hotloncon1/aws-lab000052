+++
title = "Create a new branch"
date = 2020
weight = 1
chapter = false
pre = "<b>5.1. </b>"
+++
#### Create a new branch


Now you have had hands-on experience creating and deploying AWS Lambda functions, it is time to return to our monolithic TravelBuddy application, and deploy a microservice on AWS Lambda managed through a CI/CD pipeline created with AWS CodeStar.

1. Go to [**AWS CodeStar Console**](https://console.aws.amazon.com/codesuite/codestar/home).
* Click **Projects**
* Click **Create project**
{{% notice note %}} 
Click **Create service role** if you never go to **AWS CodeStar Service** before.
{{% /notice %}}
![Deploy ImageManager Lambda Function](/images/5-use-codestar-orchestration/5.1-create-new-branch/create-new-branch-001.png?featherlight=false&width=90pc)
2. Go to **Templates** section, seclect **Java**, **Web Service** and **AWS Lambda**
* Select **Java Spring**
* Click **Next**
![Deploy ImageManager Lambda Function](/images/5-use-codestar-orchestration/5.1-create-new-branch/create-new-branch-002.png?featherlight=false&width=90pc)
3. In the **Project name** section, type ```dev-flight-svc```
* Select **Code Commit**
* Click **Next**
![Deploy ImageManager Lambda Function](/images/5-use-codestar-orchestration/5.1-create-new-branch/create-new-branch-003.png?featherlight=false&width=90pc)
4. In the **Review** page, click **Create project**
![Deploy ImageManager Lambda Function](/images/5-use-codestar-orchestration/5.1-create-new-branch/create-new-branch-004.png?featherlight=false&width=90pc)
5. Add the user **awsstudent** to the **Team** as **Owner**.
* Click **Team**
* Click **Add team member**
![Deploy ImageManager Lambda Function](/images/5-use-codestar-orchestration/5.1-create-new-branch/create-new-branch-005.png?featherlight=false&width=90pc)
6.  Go to **Team member details** section
* In the **User** section, seclect **awsstudent**
* In the **Email address** section, type your email
* In the **Project role** section, seclect **Owner**
* CLick **Allow SSH access to project instances.**
* Click **Add team member**
![Deploy ImageManager Lambda Function](/images/5-use-codestar-orchestration/5.1-create-new-branch/create-new-branch-006.png?featherlight=false&width=90pc)
7. Check added team member
![Deploy ImageManager Lambda Function](/images/5-use-codestar-orchestration/5.1-create-new-branch/create-new-branch-007.png?featherlight=false&width=90pc)
8. Go to [AWS CloudFormation Console](https://console.aws.amazon.com/cloudformation/).
* Type **aws-stack-for-Devax-lab03** to the search bar and press **Enter**.
* Click **aws-stack-for-Devax-lab03**.
![Deploy ImageManager Lambda Function](/images/5-use-codestar-orchestration/5.1-create-new-branch/create-new-branch-008.png?featherlight=false&width=90pc)
9. Click **Output**
* Save the **GitPassword** value and the **GitUserName** value
![Deploy ImageManager Lambda Function](/images/5-use-codestar-orchestration/5.1-create-new-branch/create-new-branch-009.png?featherlight=false&width=90pc)
10. In the Eclipse IDE, Find the **AWS Icon** and click it to reveal the menu
* Click **Import AWS CodeStar Project**
![Deploy ImageManager Lambda Function](/images/5-use-codestar-orchestration/5.1-create-new-branch/create-new-branch-010.png?featherlight=false&width=90pc)
11. Select the **Region** we use in this lab
* Select ```dev-flight-svc```
* Type the saved information in the step 9 into **User name** section and **Password** section
* Click **Next**
![Deploy ImageManager Lambda Function](/images/5-use-codestar-orchestration/5.1-create-new-branch/create-new-branch-011.png?featherlight=false&width=90pc)
12. Click **OK**, ignore the error **org.eclipse.egit.ui.internal.repository.tree.RepositoryTreeNodeType.getIcon()Lorg/eclipse/swt/graphics/Image;**
* Select branch **master**
* Click **Next**.
![Deploy ImageManager Lambda Function](/images/5-use-codestar-orchestration/5.1-create-new-branch/create-new-branch-012.png?featherlight=false&width=90pc)
13. Click **Finish**
![Deploy ImageManager Lambda Function](/images/5-use-codestar-orchestration/5.1-create-new-branch/create-new-branch-013.png?featherlight=false&width=90pc)
14. Click **No** to skip setup password hint
![Deploy ImageManager Lambda Function](/images/5-use-codestar-orchestration/5.1-create-new-branch/create-new-branch-014.png?featherlight=false&width=90pc)
