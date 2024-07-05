# Aws-DevOps-Project-deployment

## Code Commit👨‍💻

#### 1. Create CodeCommit Repository

create repo = demo

#### 2. Create IAM user having a permission of AWS CodeCommit
 
create iam user = demo 

HTTPS Git credentials for AWS CodeCommit , click on generate credentials and download csv.credentials file

attach a policy = AWSCodeCommitPowerUser

#### 3.  Push code to CodeCommit

open terminal

git clone < codecommi url > 

git clone --mirror https://github.com/iam-mohanty/Aws-DevOps-Project-deployment.git mycode

cd mycode

git push < code commit repo url >

## Code Build🛠️

#### 1. Setup S3 bucket for code storage

create bucket = devvvvvv1212

#### 2. Create project

create project = demoproject

Source provider = AWS CodeCommit  , repo = demo  ,  os = ubuntu

service role = codebuildservicerole

Buildspec = use a buildspec file

name = demo-build   ,  Artifacts packaging  =  .zip

create build project

start build

## Code Deploy🏗️

#### 1. create application 

application name = demo-app  

Compute platform = EC2/On-premises

#### 2. create aws codedeploy service role

go to iam and craete role

aws service = code deploy

policy = AmazonEC2FullAccess , AmazonS3FullAccess

role name = code_deploy_service_role

#### 3. create a aws ec2-service role

go to iam and create role 

aws service = ec2

policy = AmazonEC2FullAccess , AmazonEC2RoleforAWSCodeDeploy , AmazonEC2RoleforAWSCodeDeployLimited , AmazonS3FullAccess , AWSCodeDeployFullAccess , AWSCodeDeployRole

role name = ec2-service

#### 3. go to ec2 and launch the ubuntu machine

ami = ubuntu-22

name = myec2

attach that role to your ec2

connect to your ec2  --->  sudo su  ---->  apt update    ---->  vim agent.sh

```sh
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

bash agent.sh

#### 4. go to your code deploy and create a deployment group

deployment group name = demo-dep

Service role = code_deploy_service_role

Deployment type = in-place

Environment configuration  =  Amazon EC2 instances

Install AWS CodeDeploy Agent  = never

untick on enable load balancing option

click on create deployment group

#### 5. create deployment

Revision location  = s3://devvvvvv1212/demo-build

Revision file type = .zip

click on create deployment



### now go to your ec2 and click on your public ip you will see your aplication is running
