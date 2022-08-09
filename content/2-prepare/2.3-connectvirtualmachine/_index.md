+++
title = "Connect to the Windows Instance"
date = 2020
weight = 3
chapter = false
pre = "<b>2.3. </b>"
+++
#### Connect to the Windows Instance
1. Go to [**Amazon EC2 console**](https://console.aws.amazon.com/ec2/).
* Select **Intances**.
* Type ```DevAxWindowsHost``` to the search bar and press **Enter**
* Select **DevAxWindowsHost**.
* Click **Connect**.
![Connect to the Windows Instance](/images/2-prepare/2.3-connectvirtualmachine/connectvirtualmachine-001.png?featherlight=false&width=90pc)
2. In the **Connect to instance** page
* Click tab **RDP client**.
* Click **Download remote desktop file**. We will download file remote desktop to the folder contains the key pair.
* Click **Get password**.
![Connect to the Windows Instance](/images/2-prepare/2.3-connectvirtualmachine/connectvirtualmachine-002.png?featherlight=false&width=90pc)
3. In the **Get Windows password** page:
* Click **Browse**. Select file **KPforDevAxInstances.pem** we downloaded in the section 2.1.
* Click **Decrypt Password** to decrypt the password. 
![Connect to the Windows Instance](/images/2-prepare/2.3-connectvirtualmachine/connectvirtualmachine-003.png?featherlight=false&width=90pc)
4. Copy decrypted password.
![Connect to the Windows Instance](/images/2-prepare/2.3-connectvirtualmachine/connectvirtualmachine-004.png?featherlight=false&width=90pc)
5. Open file **DevAxWindowsHost.rdp** we downloaded in the step 2.
* Click **Connect**.
![Connect to the Windows Instance](/images/2-prepare/2.3-connectvirtualmachine/connectvirtualmachine-005.png?featherlight=false&width=30pc)
6. Type the password we copied in the step 4
* Click **OK**.
![Connect to the Windows Instance](/images/2-prepare/2.3-connectvirtualmachine/connectvirtualmachine-006.png?featherlight=false&width=30pc)
7. Click **Don’t ask me again for connections to this computer**.
* Click **Yes**.
![Connect to the Windows Instance](/images/2-prepare/2.3-connectvirtualmachine/connectvirtualmachine-007.png?featherlight=false&width=20pc)
8. Connect successfully. 
![Connect to the Windows Instance](/images/2-prepare/2.3-connectvirtualmachine/connectvirtualmachine-008.png?featherlight=false&width=90pc)

#### Configure AWS CLI
1. Assign the Administrator Access to user **awsstudent** was created by Cloud Formation template
* Go to [**AWS IAM Console**](https://console.aws.amazon.com/iamv2/).
* Click **Users**.
* Type ```awsstudent``` to the search bar
* Click user **awsstudent**
![Connect to the Windows Instance](/images/2-prepare/2.3-connectvirtualmachine/connectvirtualmachine-009.png?featherlight=false&width=90pc)
2. In the **Permissions policies** section
* Click **Add permissions**
![Connect to the Windows Instance](/images/2-prepare/2.3-connectvirtualmachine/connectvirtualmachine-010.png?featherlight=false&width=90pc)
3. In the **Add permissions to awsstudent** page
* Click **Attach existing policies directly**
* Type ```AdministratorAccess``` to the search bar.
* Select **AdministratorAccess**
* Click **Next:Review**
![Connect to the Windows Instance](/images/2-prepare/2.3-connectvirtualmachine/connectvirtualmachine-011.png?featherlight=false&width=90pc)
4. Click **Add Permission**
![Connect to the Windows Instance](/images/2-prepare/2.3-connectvirtualmachine/connectvirtualmachine-012.png?featherlight=false&width=90pc)
5. Click tab **Security credentials**
* Click **Create access key** to create the access key
![Connect to the Windows Instance](/images/2-prepare/2.3-connectvirtualmachine/connectvirtualmachine-013.png?featherlight=false&width=90pc)
6. Click **Download .csv file** to save the **Access key** and the **Secret access key** to use in the next steps
![Connect to the Windows Instance](/images/2-prepare/2.3-connectvirtualmachine/connectvirtualmachine-014.png?featherlight=false&width=90pc)
7. Execute the following command:
```
aws configure set profile.devaxacademy.region <your_region>
aws configure set profile.devaxacademy.aws_access_key_id <access_key_id>
aws configure set profile.devaxacademy.aws_secret_access_key <secret_access_key>
```
{{% notice note %}} 
Replace **<your_region>** with your **Region code** \
Replace **<access_key_id>** with **Access Key Id** we saved in the step 6\
Replace **<secret_access_key>** with **Secret Access Key** we saved in the step 6
{{% /notice %}}
![Connect to the Windows Instance](/images/2-prepare/2.3-connectvirtualmachine/connectvirtualmachine-015.png?featherlight=false&width=60pc)

#### Configure Eclipse IDE
1. Open **Eclipse IDE** in the Windows virtual machine.
* Click **Use this as the default and do not ask again**
* CLick **Launch**
![Connect to the Windows Instance](/images/2-prepare/2.3-connectvirtualmachine/connectvirtualmachine-016.png?featherlight=false&width=50pc)
2. Click **Finish**
![Connect to the Windows Instance](/images/2-prepare/2.3-connectvirtualmachine/connectvirtualmachine-017.png?featherlight=false&width=60pc)
3. Click **Restore** button. 
![Connect to the Windows Instance](/images/2-prepare/2.3-connectvirtualmachine/connectvirtualmachine-018.png?featherlight=false&width=90pc)
4. find the **AWS Icon**, click it to reveal the menu.
* Click **Preferences...**
![Connect to the Windows Instance](/images/2-prepare/2.3-connectvirtualmachine/connectvirtualmachine-019.png?featherlight=false&width=90pc)
5. In the **Default Profile** section, Select ```devaxacademy```
* Click **Apply and Close**
![Connect to the Windows Instance](/images/2-prepare/2.3-connectvirtualmachine/connectvirtualmachine-020.png?featherlight=false&width=90pc)