# Python App Deployment Via CICD-Jenkins

## Introduction 

# Step 1 : Create Python-App Server
Lunch Instnace

- Name and tags : Python-App-Server
- Os Images - Ubuntu
- Instance type - t2.micro
- Security group - Existing One
- Lunch Instnace

![project screenshot](/Images/server.PNG)

Also enable 5000 port in security group and For jenkins server you have to enable 8080 port

![project screenshot](/Images/Sg.PNG)

## Step 2 : Copy .pem file to Jenkins server
Now go to the where your .pem key is located and copy that file to your jenkin server by using below cmd this cmd run on gitbash
```
scp -i <key-file-name> <key-file-name> username@public-ip:defaultpath

example :
scp -i jenkins-file.pem jenkins-file.pem ubuntu@54.81.24.101:/home/ubuntu
```
## Step 3 : Install required plugins
In the jenkins you have to install some plugins for build CICD pipeline Fr that you have to follow below steps if you alredy installed then skip this step. 

- Log in into jenkins server
- Click on Setting
- Open plugins --> Available plugins
- Search
    - ssh agen --> install
    - pipeline --> install
    - ssh build agent --> install
    - github --> install
- Now restart the jenkins.

## Step 4 : Create Job For python-app deployment
- Click on "New Item"
- Enter an item name : NodeJs-Deployment
- Select an item type : pipeline
- Click on Ok

![project screenshot](/Images/job.png)

For Python-App application code you take from this repository https://github.com/Vishu-Nargunde99/PythonApp-Deploy-CICD-Jenkins 

You can clone this repo on your local machine and upload to your GitHub account. This code we can use anytime when you want to install nodejs application.

## Step 5 : Step 6 : Create Credential
- Click on Setting
- In security section --> Credential --> Click on global --> Add Credential
    - Kind - SSH Username with private key
    - Scope - Global(jenkins, nodes, items)
    - ID - ssh-key-credential
    - Description - ssh-key-credential
    - Username - ubuntu
    - Private Key - Check in - Entire directly
    - Click on Add
    - Copy your private-key from your local machine
    - Create 
Here our credential is created

![project screenshot](/Images/credential.PNG)

## Step 6 : Configure Job
- Go to created job "PythonApp-Deployment"
- click on Configure

![project screenshot](/Images/configire.png)

- Scoll down
- In trigger section --> check in - GitHub hook trigger GITScm polling
- Pipeline
    - Definition - Pipeline script from SCM
        - SCM - Git
            - Repository - Copy your repository HTTPS where your jenkinfile stored
            - Branch - main/master (which is your branch name)
        - Script Path - Change Jenkinsfile To jenkinfile
- Click on "Save".
- Click on "Build Now"

![project screenshot](/Images/build%20succ.PNG)

Here build now is successfull.

1

## Step 7 : Generate Automate code push
- Log in into your GitHub Account
- Go to your code repository of Nodejs
- Click on setting
- Click on webhook
- Add webhook
- payload URl
```
http://<jenkins-server-public-ip>:8080/github-webhook/

Example
http://54.81.24.101:8080/hithub-webhook/
```
- Add webhook
- Now you can just chnage your code then add, commit and push code to your reposotory.













