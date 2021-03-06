# Packer Templates for Response Management, Respondent Home and Supporting Services
This repository contains [Packer](https://www.packer.io/) templates for building:

* [Response Management](https://github.com/ONSdigital/response-management-service)
* [Response Operations](https://github.com/ONSdigital/response-management-ui)
* [Respondent Home](https://github.com/ONSdigital/respondent-home-ui)
* A populated OpenLDAP server
* An SSH bastion host
* A support client host
* A base CentOS 7 operating system stack

The Packer templates create an Amazon Machine Image (AMI) that can be used to quickly launch EC2 instances containing one of the pre-built stacks listed above. [Chef](https://chef.io/) is used by Packer as a provisioner to install and configure the application software within the AMI in a consistent and repeatable fashion.

## Prerequisites
The Chef validation key (`validation.pem`) and encrypted data bag secret (`encrypted_data_bag_secret`) must be present on the machine on which Packer is running in the `/etc/chef` directory and be readable by the operating system account used to run Packer. [Knife](https://docs.chef.io/knife.html) should be installed and configured on the machine on which Packer is running in order for Packer to be able to properly clean up the Chef node and client.

Create a file named `aws-variables.json` within the same directory as this README file. This is a file that provides Packer with values for the Chef environment (development, test, production etc.), AWS access key, secret key, region, source AMI, subnet and VPC variables as shown below. Note that this file is ignored by Git.

```json
{
  "aws_access_key": "...",
  "aws_secret_key": "...",
  "chef_environment": "...",
  "region": "eu-west-1",
  "source_ami": "...",
  "subnet_id": "subnet-...",
  "vpc_id": "vpc-..."
}
```

## Building Response Management
Edit the path `/home/centos/code/response-management-service` on the first line of `response-management.sh` to match the path to the Response Management Service Git repository on your machine so that the Git commit SHA can be stored in an environment variable ready for Packer to insert it into the AMI description. Build the AMI using:

  `./response-management.sh`

## Building Response Operations (Response Management UI)
Edit the path `/home/centos/code/response-management-ui` on the first line of `response-operations.sh` to match the path to the Response Management UI Git repository on your machine so that the Git commit SHA can be stored in an environment variable ready for Packer to insert it into the AMI description. Build the AMI using:

  `./response-operations.sh`

## Building Respondent Home
Edit the path `/home/centos/code/respondent-home-ui` on the first line of `respondent-home.sh` to match the path to the Respondent Home Git repository on your machine so that the Git commit SHA can be stored in an environment variable ready for Packer to insert it into the AMI description. Build the AMI using:

  `./respondent-home.sh`

## Building OpenLDAP
A Packer template is included for building an OpenLDAP stack pre-populated with the Response Operations users and groups. Build the OpenLDAP AMI using:

  `./openldap.sh`

## Building SSH Bastion
A Packer template is included for building an SSH bastion host. Build the SSH bastion AMI using:

  `./ssh-bastion.sh`

## Building Support Client
A Packer template is included for building a support client host that includes a PostgreSQL client and a Ruby script for creating samples within Response Management. Build the support client AMI using:

  `./support-client.sh`

## Building Base OS
A Packer template is included for building a CentOS 7 base OS stack that includes some `/etc/hosts` entries required by some of the Chef cookbooks used by the templates above. This AMI is used as the source AMI by the other templates. Accordingly its AMI ID must be specified in the `aws-variables.json` file described above. Build the Base OS AMI using:

  `./base-os.sh`
