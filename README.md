# Packer Templates for Response Management and Respondent Home
This repository contains [Packer](https://www.packer.io/) templates for building [Response Management](https://github.com/ONSdigital/response-management-service) and [Respondent Home](https://github.com/ONSdigital/respondent-home-ui). The Packer templates create an Amazon Machine Image (AMI) that can be used to quickly launch EC2 instances containing a pre-built Response Management stack or Respondent Home stack. [Chef](https://chef.io/) is used by Packer as a provisioner to install and configure the application software within the AMI in a consistent and repeatable fashion.

## Prerequisites
The Chef validation key (`validation.pem`) and encrypted data bag secret (`encrypted_data_bag_secret`) must be present on the machine on which Packer is running in the `/etc/chef` directory and be readable by the operating system account used to run Packer. Knife should be installed and configured on the machine on which Packer is running in order for Packer to be able to properly clean up the Chef node and client.

Create a file named `aws-variables.json` within the same directory as this README file. This is a file that provides Packer with values for the Chef environment (development, test, production etc.), AWS access key, secret key, region, subnet and VPC variables as shown below. Note that this file is ignored by Git.

```json
{
  "aws_access_key": "...",
  "aws_secret_key": "...",
  "chef_environment": "...",
  "region": "eu-west-1",
  "subnet_id": "subnet-...",
  "vpc_id": "vpc-..."
}
```

## Building Respondent Home
Edit the path `/home/centos/code/respondent-home-ui` on the first line of `respondent-home.sh` to match the path to the Respondent Home Git repository on your machine so that the Git commit SHA can be stored in an environment variable ready for Packer to insert it into the AMI description. Build the AMI using:

  `./respondent-home.sh`
