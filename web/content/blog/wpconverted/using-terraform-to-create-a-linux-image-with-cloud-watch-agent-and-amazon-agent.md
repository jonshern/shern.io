---
title: 'Using Terraform to Create a Linux Image with Cloud Watch Agent and Amazon Agent'
date: Mon, 03 Apr 2019 04:10:03 +0000
draft: false
tags: ['terraform', 'Uncategorized']
---

In this installment of my adventures with Terraform.  I want to spin up an Amazon Linux AMI with the Cloud Watch Agent and the Amazon Agent.

  

The Cloud Watch Agent will configure the ability to send the logs for the EC2 Instance to Cloud Watch.

  

The Amazon Agent will enable Amazon Inspector to inspect the instance for security vulnerabilities.

  

The first step is to create a Terraform script that remotely executes a shell script.

  

Three parts are required.

**Connection**

This provides the ssh connection info in order to connect to the EC2 Instance.

\[code\] connection { user = "${var.INSTANCE\_USERNAME}" private\_key = "${file("${var.PATH\_TO\_PRIVATE\_KEY}")}" } \[/code\]

**File Provisioner **

This allows us to take an arbitraty files and upload it to the EC2 instance, we could just put all of the scripts in the remote-exec, but that would be ugly.

\[code\] provisioner "file" { source = "awscli.conf" destination = "/tmp/awscli.conf" } provisioner "remote-exec" { inline = \[ "chmod +x /tmp/script.sh", "sudo /tmp/script.sh", "sudo cp /tmp/awscli.conf /etc/awslogs/awscli.conf", \] } \[/code\]

**Remote Exec Provisioner**

Create a remote ssh session for executing commands on the EC2 Instance.

\[code\] provisioner "remote-exec" { inline = \[ "chmod +x /tmp/script.sh", "sudo /tmp/script.sh", "sudo cp /tmp/awscli.conf /etc/awslogs/awscli.conf", \] } \[/code\]

So clearly there is some magic missing.  I mean what am i actually deploying?

For that we go to the Shell Script

First part install the Logging Agent

\[code\] sudo yum update -y sudo yum install -y awslogs sudo service awslogs start sudo chkconfig awslogs on \[/code\]

Here are the amazon instructions

http://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/QuickStartEC2Instance.html

Second part install the amazon agents

\[code\] wget https://d1wk0tztpsntt1.cloudfront.net/linux/latest/install sudo bash install sudo /etc/init.d/awsagent start sudo /opt/aws/awsagent/bin/awsagent status \[/code\]

Here are the amazon instructions

http://docs.aws.amazon.com/inspector/latest/userguide/inspector\_agents-on-linux.html

When you run terraform apply you get a lot of output.

Now we can jump into amazon inspector, use a filter for a tag and see what we find.

\[code\] aws\_instance.linuxec2 (remote-exec): Total download size: 78 k aws\_instance.linuxec2 (remote-exec): Installed size: 240 k aws\_instance.linuxec2 (remote-exec): Downloading packages: aws\_instance.linuxec2 (remote-exec): (1/2): aws-cli-p | 69 kB 00:00 aws\_instance.linuxec2 (remote-exec): (2/2): awslogs-1 | 8.8 kB 00:00 aws\_instance.linuxec2 (remote-exec): ---------------------------------------- aws\_instance.linuxec2 (remote-exec): Total 436 kB/s | 78 kB 00:00 aws\_instance.linuxec2 (remote-exec): Running transaction check aws\_instance.linuxec2 (remote-exec): Running transaction test aws\_instance.linuxec2 (remote-exec): Transaction test succeeded aws\_instance.linuxec2 (remote-exec): Running transaction aws\_instance.linuxec2 (remote-exec): Installing : aws-cli- \[ \] 1/2 aws\_instance.linuxec2 (remote-exec): Installing : aws-cli- \[# \] 1/2 aws\_instance.linuxec2 (remote-exec): Installing : aws-cli- \[#### \] 1/2 aws\_instance.linuxec2 (remote-exec): Installing : aws-cli- \[##### \] 1/2 aws\_instance.linuxec2 (remote-exec): Installing : aws-cli- \[###### \] 1/2 aws\_instance.linuxec2 (remote-exec): Installing : aws-cli- \[######## \] 1/2 aws\_instance.linuxec2 (remote-exec): Installing : aws-cli-plugin-clo 1/2 aws\_instance.linuxec2 (remote-exec): Installing : awslogs- \[ \] 2/2 aws\_instance.linuxec2 (remote-exec): Installing : awslogs- \[##### \] 2/2 aws\_instance.linuxec2 (remote-exec): Installing : awslogs- \[###### \] 2/2 aws\_instance.linuxec2 (remote-exec): Installing : awslogs- \[######## \] 2/2 aws\_instance.linuxec2 (remote-exec): Installing : awslogs-1.1.2-1.10 2/2 aws\_instance.linuxec2 (remote-exec): Verifying : awslogs-1.1.2-1.10 1/2 aws\_instance.linuxec2 (remote-exec): Verifying : aws-cli-plugin-clo 2/2 aws\_instance.linuxec2 (remote-exec): Installed: aws\_instance.linuxec2 (remote-exec): awslogs.noarch 0:1.1.2-1.10.amzn1 aws\_instance.linuxec2 (remote-exec): Transaction Summary aws\_instance.linuxec2 (remote-exec): ======================================== aws\_instance.linuxec2 (remote-exec): Install 1 Package aws\_instance.linuxec2 (remote-exec): Total size: 5.9 M aws\_instance.linuxec2 (remote-exec): Installed size: 5.9 M aws\_instance.linuxec2 (remote-exec): Downloading packages: aws\_instance.linuxec2 (remote-exec): Running transaction check aws\_instance.linuxec2 (remote-exec): Running transaction test aws\_instance.linuxec2 (remote-exec): Transaction test succeeded aws\_instance.linuxec2 (remote-exec): Running transaction aws\_instance.linuxec2 (remote-exec): Installing : AwsAgent \[ \] 1/1 aws\_instance.linuxec2 (remote-exec): Installing : AwsAgent \[# \] 1/1 aws\_instance.linuxec2 (remote-exec): Installing : AwsAgent \[## \] 1/1 aws\_instance.linuxec2 (remote-exec): Installing : AwsAgent \[### \] 1/1 aws\_instance.linuxec2 (remote-exec): Installing : AwsAgent \[#### \] 1/1 aws\_instance.linuxec2 (remote-exec): Installing : AwsAgent \[##### \] 1/1 aws\_instance.linuxec2 (remote-exec): Installing : AwsAgent \[###### \] 1/1 aws\_instance.linuxec2 (remote-exec): Installing : AwsAgent \[####### \] 1/1 aws\_instance.linuxec2 (remote-exec): Installing : AwsAgent \[######## \] 1/1 aws\_instance.linuxec2 (remote-exec): Installing : AwsAgentKernelModu 1/1 aws\_instance.linuxec2 (remote-exec): Verifying : AwsAgentKernelModu 1/1 aws\_instance.linuxec2 (remote-exec): Installed: aws\_instance.linuxec2 (remote-exec): AwsAgentKernelModule\_\_amzn\_\_4.4.41-36.55.amzn1.x86\_64 0:1.0.27.1-0 \[/code\]

Here is a link to the github repo with the working code.

https://github.com/jonshern/awsinspectorpoc

Look in the folder ec2instances.