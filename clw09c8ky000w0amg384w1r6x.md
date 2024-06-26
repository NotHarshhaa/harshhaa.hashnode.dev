---
title: "AWS DevOps Real-Time Deployment - Dev → Pre-PROD → Production"
datePublished: Fri May 10 2024 05:49:28 GMT+0000 (Coordinated Universal Time)
cuid: clw09c8ky000w0amg384w1r6x
slug: aws-devops-real-time-deployment-dev-pre-prod-production
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1716784312553/8538933b-dc45-459c-99ab-feb0883d7207.png
tags: aws, realtime, development, deployment, devops, production, devops-articles, aws-devops

---


## **Completion Steps →**

1. fetch a code from GitHub by **git clone** [**https://github.com/NotHarshhaa/AWS-DevOps\_Real-Time\_Deployment.git**](https://github.com/NotHarshhaa/AWS-DevOps_Real-Time_Deployment.git)
    
2. **Setup code commit repository on AWS**
    
3. push code to code commit using **AWS CLI**
    
4. setup **aws code-build** for building code
    
5. setup **S3 bucket** for code storage
    
6. setup **code deployment** for the deployment of code stored in AWS S3
    
7. setup an **EC2** in which you deployed your application
    
8. Setting Up AWS CodeDeploy Agent on Ubuntu EC2
    
9. create the deployment group and deploy the application
    
10. Setup **code pipeline** to automate the whole process
    

## **Step 1 →**

1. git clone [**https://github.com/NotHarshhaa/AWS-DevOps\_Real-Time\_Deployment.git**](https://github.com/NotHarshhaa/AWS-DevOps_Real-Time_Deployment.git)
    

above command pulls your application code to your local that is needed to be pushed in the aws code commit

## **step 2 →Setup code commit**

1. go to the AWS console search aws code commit and create a demo repository
    

![](https://miro.medium.com/v2/resize:fit:736/1*3aj5r0irsN81qbR20QTN8Q.png align="center")

2\. provide necessary permission to aws code commit to use it with aws cli which needs to create an IAM user having the permission of aws code commit

3\. search IAM and click on Create user

![](https://miro.medium.com/v2/resize:fit:736/1*ivKfcfRcx5g3gac4aviIjQ.png align="center")

4\. click next and create a user

5\. now you have to permit your user to code commit

![](https://miro.medium.com/v2/resize:fit:736/1*dXhIWje_EpwuhlEP1r8KlQ.png align="center")

6 . **click on user →security credentials →scroll down for HTTPS Git credentials for AWS CodeCommit →click on generate credentials and download csv.credentials file**

![](https://miro.medium.com/v2/resize:fit:736/1*wKCDmcF8_96BnGDEd_Dwsw.png align="center")

7\. you also need to attach a policy called “[**AWSCodeCommitPowerUser**](https://us-east-1.console.aws.amazon.com/iamv2/home?region=us-east-1#/policies/details/arn%3Aaws%3Aiam%3A%3Aaws%3Apolicy%2FAWSCodeCommitPowerUser)**”**

go to permissions and then click on attach policies directly search for the above policy and click on add

![](https://miro.medium.com/v2/resize:fit:736/1*gChrpxkoHIl0a9HTcNxmjw.png align="center")

your code commit repository is set next move to step

## **Step 3 → push code to code commit using AWS CLI**

1. clone your code commit URL go to your terminal and run git clone &lt;copied url&gt;
    
2. it will ask you for a username and password → give it to your code commit credentials that you generated
    

![](https://miro.medium.com/v2/resize:fit:736/1*djNGdNR2PM9B85E40zcoOA.png align="left")

3\. now you have to push all the cloned code of the project repository to your code commit repository by commands

1. cd demo
    
2. git clone [https://github.com/NotHarshhaa/AWS-DevOps\_Real-Time\_Deployment.git](https://github.com/NotHarshhaa/AWS-DevOps_Real-Time_Deployment.git)
    
3. git add.
    
4. git commit -m ”new commit”
    
5. git push
    

![](https://miro.medium.com/v2/resize:fit:736/1*JPbfqX9S0dtvZQQkdGfosA.png align="left")

pushed code in a code commit

## **Step 3 → Setup aws code-build for building code**

1. go to the AWS console and search for AWS code build
    
2. click on the Create Project option
    
3. give a name to your project **“demo project”**
    
4. on the repo option **click on demo** and **branch to master**
    
5. **for OS choose Ubuntu,runtime to standard and image to the latest version**
    
6. **build spec file →** It is a file that tells the code build what steps are needed when you building the code we have a build spec file in the code commit so the “click **on use a build spec file”** option
    
7. click on **create build project** option and keep everything as it is
    

![](https://miro.medium.com/v2/resize:fit:736/1*MwEcKLXS7gRU08fAD7GcQg.png align="left")

![](https://miro.medium.com/v2/resize:fit:736/1*ivoMns33TnQh9WFavRNTzA.png align="left")

![](https://miro.medium.com/v2/resize:fit:736/1*Dpj9jsA2fQKTb3YMAezF7w.png align="left")

![](https://miro.medium.com/v2/resize:fit:736/1*v5HTslVyTbPXg5dXLIGZNA.png align="left")

8\. Now click on start **build option**

![](https://miro.medium.com/v2/resize:fit:736/1*wNoxRaL8Eqo5r1dD9I4XYw.png align="left")

**now you need to store this code in s3 which is further used by code deployment for deployment**

## **step 5 →setup S3 bucket for code storage**

1. go to the console and **search for s3 and create a bucket**
    
2. bucket name **should be unique globally**
    

![](https://miro.medium.com/v2/resize:fit:736/1*Vtkw74b9Cm_d97bSdveuzA.png align="left")

**3\. go to code build and click on edit → artefact →change no artefact to s3 and choose a bucket created in above step**

4\. and **choose a zip file** for storage

![](https://miro.medium.com/v2/resize:fit:736/1*QADTmX9F8aUv7ac0X6U9fQ.png align="left")

5\. **click on update artefacts and build again the same project with this extra step of artifacts**

6\. after building you will see that **your code is stored in an s3 bucket**

***now we have to move to step no.6***

## **Step 6 →setup code deployment for deployment of code stored in aws S3 to an EC2**

1. go to console and search for aws code deploy and **click on create application option**
    

![](https://miro.medium.com/v2/resize:fit:736/1*ktCk_5yzT7RCXBLjJIHVwQ.png align="left")

select ec2 in the dropdown menu

## **Step 7 → setup an EC2 in which the application will run or be deployed by code deploy**

1. **go to ec2 and launch the Ubuntu machine**
    

### **create an aws ec2-service role**

**having the following permission which helps ec2 to communicate with code deploy**

![](https://miro.medium.com/v2/resize:fit:736/1*Zs7GlDIaaMMUS8zwbuQPag.png align="left")

![](https://miro.medium.com/v2/resize:fit:736/1*6DnLdOAjAC02IbkeYxl8pw.png align="left")

2\. attach that role to your ec2 →actions →security →modify iam role

### **create an aws code\_deploy\_service\_role**

![](https://miro.medium.com/v2/resize:fit:736/1*uLnOg9XOn5qbtqwAPj8sMw.png align="left")

this role needs to be attached on code deployment to allow it to make contact with ec2 and s3

## **Step 8 →Setting Up AWS CodeDeploy Agent on Ubuntu EC2**

To deploy your app to EC2, CodeDeploy needs an agent that deploys the code on your EC2.

So let’s set it up.

Create a shell script with the below contents and run it

1. connect to your ec2 and run the following commands
    
2. sudo su
    
3. apt update
    
4. vim [agent.sh](http://agent.sh)
    

copy and paste the following code to your file

```bash
#!/bin/bash
# This installs the CodeDeploy agent and its prerequisites on Ubuntu 22.04.
sudo apt-get update
sudo apt-get install ruby-full ruby-webrick wget -y
cd /tmp
wget https://aws-codedeploy-us-east-1.s3.us-east-1.amazonaws.com/releases/codedeploy-agent_1.3.2-1902_all.deb
mkdir codedeploy-agent_1.3.2-1902_ubuntu22 
dpkg-deb -R codedeploy-agent_1.3.2-1902_all.deb codedeploy-agent_1.3.2-1902_ubuntu22 
sed 's/Depends:.*/Depends:ruby3.0/' -i ./codedeploy-agent_1.3.2-1902_ubuntu22/DEBIAN/control 
dpkg-deb -b codedeploy-agent_1.3.2-1902_ubuntu22/ 
sudo dpkg -i codedeploy-agent_1.3.2-1902_ubuntu22.deb
systemctl list-units --type=service | grep codedeploy 
sudo service codedeploy-agent status
```

5\. press →**esc + : +wq** →for exit

6\. to run this file use the command →**bash** [**agent.sh**](http://agent.sh)

you have the following output

![](https://miro.medium.com/v2/resize:fit:736/1*6tO6_mBewNK_Ft8PwxNmOQ.png align="left")

your code deploy agent is running

let’s move to the next step

## **Step 9 →create the deployment group and deploy the application**

1. go to your code deploy and create a deployment group
    

![](https://miro.medium.com/v2/resize:fit:736/1*_O009dlcWvZp1_yf_Z1LTg.png align="left")

![](https://miro.medium.com/v2/resize:fit:736/1*en1HNZ14zelwrCrDNnt9Yw.png align="left")

copy the srn of aws code\_deploy\_service\_role or created above

2\. choose **in place** in deployment type

![](https://miro.medium.com/v2/resize:fit:736/1*o_Ju_nntKfAV49JIyMCEQQ.png align="left")

**3\. select ec2 instances →name →choose your ec2 in which code deploy agent is running**

![](https://miro.medium.com/v2/resize:fit:736/1*LXmPsUFuj50i9wX_ogmkWQ.png align="left")

4\. untick on enable load balancing option and never on install aws code deploy agent

and click on the Create Deployment option you will see the following page

![](https://miro.medium.com/v2/resize:fit:736/1*YppXg7-qTs6sDMkQYJ7M6w.png align="left")

click on the Create Deployment option following page appears

![](https://miro.medium.com/v2/resize:fit:736/1*KpptYbjCU2moQz0mvDOtAQ.png align="left")

5\. **select the above options and paste the path of your build code that is stored in s3 just go to s3 choose a folder and copy the s3 URL**

and then click on **Create Deployment after a few seconds**

![](https://miro.medium.com/v2/resize:fit:736/1*LE8ZoaLJRjM8vOYeCku4jw.png align="left")

your application is successfully deployed

**now go to your ec2 and click on your public IP you will see your application is running**

![](https://miro.medium.com/v2/resize:fit:736/1*N67t9_BuiithyFJoW0tUXg.png align="left")

congrats you are a DevOps and cloud engineer both at the same time

## **Step 10 → Setup code pipeline to automate the whole process**

1. go to your code pipeline and **create a pipeline**
    

![](https://miro.medium.com/v2/resize:fit:736/1*fkPjQQ2r3xKs_vpHhoWhDw.png align="left")

2\. **select pipeline type = V2**

click on new service role →next step

![](https://miro.medium.com/v2/resize:fit:736/1*mnH1dwccsx6TS30nq7yPBg.png align="left")

3\. **choose source = code commit**

**repo=demo**

**branch=master**

**choose aws code pipeline(this options immediatley triggerd pipeline whenever there is a new commit in a repo)**

![](https://miro.medium.com/v2/resize:fit:736/1*Xw-rav6dNwz_-ybrNupBGg.png align="left")

4\. choose aws code build

project name =demobuild →single build →next step

![](https://miro.medium.com/v2/resize:fit:736/1*TOwf41L6RhDWIocsd163ZA.png align="left")

5\. choose aws code deploy

aplication name = demo app

deployment group=demo\_dep

next

6\. review and pipeline and start it

![](https://miro.medium.com/v2/resize:fit:736/1*k_eznVbHnCWeBO9_oE10Cw.png align="left")

7\. for checking if it triggers or not whenever i commit make some changes in file and now the result is the following image

![](https://miro.medium.com/v2/resize:fit:736/1*0z_pCxUHOG51xW9oOBQeLg.png align="left")

If you find this article helpful then you can [**buy me a coffee**](https://www.buymeacoffee.com/harshhaareddy)**.**

Follow for more stories like this 😊/ [**GitHub**](https://github.com/NotHarshhaa)**.**