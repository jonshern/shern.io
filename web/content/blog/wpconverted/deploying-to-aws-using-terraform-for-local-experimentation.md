---
title: 'Deploying to AWS using Terraform for Local Experimentation'
date: Mon, 20 Mar 2017 10:30:23 +0000
draft: false
tags: ['cloud', 'terraform']
---

Terraform is a tool used to turn an api into code, and in this case we are going to use with AWS. The download of terraform is available [here](https://www.terraform.io/downloads.html) You don't want aws secrets in your git repo, so this is a basic structure i start out with when i am doing **local** terraform experimentation. ![](https://www.terraform.io/assets/images/logo_large-3e11db19.png)

### **Terraform Setup on Ubuntu**

There is a nice script here that installs terraform.  Just run it periodically to keep it up to date. https://github.com/ryanmaclean/awful-bash-scripts/blob/master/install\_recent\_terraform\_packer.sh ![](https://pbs.twimg.com/media/CyONT7bW8AA_N1w.jpg) I am a noob in the linux stuff so i needed to google to how to make it executable. \[code\] sudo chmod +x scriptname \[/code\]

### **vars.tf**

Used to define the set of variables used in Terraform \[code\] variable "AWS\_ACCESS\_KEY" { } variable "AWS\_SECRET\_KEY" { } variable "AWS\_REGION" { default = "us-east-1" } \[/code\]   **provider.tf**

Defines what api will get ran. Since this is aws we are deploying to we are using the aws provider.  If we were deploying to azure the provider would be azurerm \[code\] provider "aws" { access\_key = "${var.AWS\_ACCESS\_KEY}" secret\_key = "${var.AWS\_SECRET\_KEY}" region = "${var.AWS\_REGION} } \[/code\]

### **terraform.tfvars**

Put this in the git ignore so the secrets stay on your local machine. If we were using a build server an alternative solution would need to be used.  But for a small scripting effort this works great. \[code\] AWS\_ACCESS\_KEY = "" AWS\_SECRET\_KEY = "" AWS\_REGION = "" \[/code\]

### **instance.tf**

This one can really be named anything. It is where all of the resources would get built out. \[code\] resource "aws\_instance" "example" { ami = "" instance\_type = "t2.micro" } \[/code\]

Here is the github repo that this is stored in. [https://github.com/jonshern/terraformstarter](https://github.com/jonshern/terraformstarter)