
# Configuring the Jenkins on an EC2 Instance (AWS)

I used these steps to configure Jenkins on an EC2 instance. You can follow these to the same. In case of some improvement/ update, please do let me know. It will be beneficial for all of us.



## Step 1: Creating EC2 and configuring it

We will do this in two steps. In the first step, we will launch an instance on AWS and configure it, while in the second step, we will do some configuration on the Jenkins server side so that our EC2 instance can connect and install the required plugins among other things settings.

Step 1: Create a Security Group and a Key

SG inbound rule(s):

• Click Add Rule, and then choose SSH from the Type list. Under Source, select Custom and in the text box enter <public IP address range that you decided on in step 1>/32 e.g. 110.20.20.20/32.

• Click Add Rule, and then choose HTTP from the Type list

• Click Add Rule, and then choose Custom TCP Rule from the Type list. Under Port Range enter 8080.



Then launch an ec2 instance with pre-defined key and attach the above defined SG with it (note: SG and Key can be defined while creating an instance).


Now, connect the virtual machine through SSH.



### Process of installing Jenkins on a launched EC2

• To check that your software packages on your instance are up to date, enter the following command to complete a rapid software upgrade


```bash
  sudo yum update -y
```

•	Use the command line to add the Jenkins repository
```bash
  sudo wget -0 /etc/yum.repos.d/Jenkins.repo \ 
https://pkg.jenkins.io/redhat-stable/jenkins.rep

```

•	Import a key file from Jenkins-CI to enable package installation

```bash
  sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io.key

  sudo yum upgrade 
```

•	Installing Java
```bash
  sudo amazon-linux-extras install java-openjdk11 -y
```

•	Enable the EPEL repository, which is already installed on the original version of Amazon Linux on AWS (dependency for AWS)
```bash
  yum install amazon epel-release
```

•	Installing Jenkins
```bash
  sudo yum install jenkins -y
```

•	Enable the Jenkins service to start at boot
```bash
  sudo systemctl enable Jenkins
```

•	Initialize Jenkins as a service
```bash
  sudo systemctl start Jenkins
```

•	Status check of the Jenkins service
```bash
  sudo systemctl status Jenkins
```

## Step 2: Configuring Jenkins

Jenkins is now deployed and operational on your Amazon EC2 instance. To configure Jenkins, navigate to http://<server public DNS>:8080 in your preferred browser. Jenkins will be accessible via its administration interface.

•	As prompted, enter the password found at

```bash
   /var/lib/jenkins/secrets/initialAdminPassword
```


•	Command to display the password

```bash
  sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

•	The Jenkins installation script will bring you to the Customize Jenkins page. Install recommended plugins as selected. When the installation is finished, create the First Admin user, and then Save and Continue. Then, on the left, choose Manage Jenkins, followed by Manage Plugins.

•	Further, click on the Available tab, and then enter Amazon EC2 plugin at the top right. Select the checkbox next to Amazon EC2 plugin, and then click Install without restart. Once the installation is done, click Back to Dashboard. Then click on Configure a cloud. Now, you can click Add a new cloud, and select Amazon EC2. A collection of new fields appears.

•	Next, go to the Available tab and input Amazon EC2 plugin at the top right. Select the Amazon EC2 plugin checkbox, then click Install without restart. When the installation is complete, return to the dashboard by clicking Back to Dashboard. Then choose Configure a cloud. You may now click add a new cloud and choose Amazon EC2. Plenty of fresh fields will popup.

•	Complete all of the fields.

#### Note: Please keep in mind that you will need to add credentials of your AWS account.

```bash
  The End. Thank you!
```

### References:

https://aws.amazon.com/premiumsupport/knowledge-center/ec2-enable-epel/

https://cloudaffaire.com/how-to-install-jenkins-in-aws-ec2-instance/

https://www.jenkins.io/doc/tutorials/tutorial-for-installing-jenkins-on-AWS/

https://dev.to/aws-builders/jenkins-installation-configuration-on-aws-ec2-linux-instance-3npl

