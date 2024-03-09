. Creating our own GIT Server on AWS
. Setting Developer SSH Setup
. Pushing and Pulling to Remote Repository
. Git Web UI
. How do Git Store Files?

-------------------------------------------------------------------------------------------------

Terraform script to create an EC2 instance in the us-east-1 region with Ubuntu AMI, t2.medium instance type, and 15GB of storage.

main.tf

provider "aws" {
  region = "us-east-1"
}

resource "aws_instance" "web" {
  ami           = "ami-0c55b159cbfafe1f0" // Ubuntu 20.04 LTS AMI for us-east-1
  instance_type = "t2.medium"
  key_name      = "aws-git-key"
  subnet_id     = "subnet-12345678" // Replace with your subnet ID

  root_block_device {
    volume_size = 15
  }

  tags = {
    Name = "demo-github"
  }
}

---------------------------------------------------------------------------------------
connect with ec2 instace through SSh 

chown 400 "aws-git-key.pem"

ssh -i "aws-git-key.pem" ubuntu@ec2-44-203-70-164.compute-1.amazonaws.com

sudo apt update && sudo apt install git -y

git --version

sudo adduser git

sudo su git

cd /var/lib/git

sudo chown -R git /var/lib/git //this git permission to make folder in /var/lib/git

mkdir my-review-app.git

cd my-review-app.git

git init --bare //this will make folder only records chnages not add chnges files [make folder like github]

to clone the repo  into your local sytem we have to add our system ssh file into it 

cd /home/ubuntu

mkdir .ssh
vim authorized_keys //add your ssh public key into it {id_rsa.pub}

=>not you can use git clone 
 git clone git@44.203.70.164:/var/lib/git/my-review-app.git
 >create some files and push it 
 git add .
 git commit -m "first comit"
 git push 
>check remote
git remote -v
 >check logs
 git log
 >check diff
 git diff 1e141f56126f12 g2uu2yy23ggubkjqji

=>to add ui we have add Ruby into it 

sudo apt install ruby -y
sudo apt instal gitweb -y

=>Add github ui into it with you can see all thing like git tree,commitdiff, logs
github instaweb --httpd=webrick



