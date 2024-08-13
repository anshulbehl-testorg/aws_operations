# AWS Operations Role: `aws.aws_operations.run`

## Overview

The `aws.aws_operations.run` Ansible role is designed to manage and automate AWS infrastructure tasks such as creating a Virtual Private Cloud (VPC), subnets, Internet Gateway (IGW), security groups, and EC2 instances. It also includes teardown logic to clean up the created resources when necessary.

This role integrates with survey variables, allowing for dynamic input of critical parameters during playbook execution. The `vpc_cidr` and `subnet_cidr` are fixed variables defined within the playbook, while other essential parameters are provided through user input via surveys.

## Variables

### Survey Variables

The following variables are expected to be provided via surveys when the role is executed:

- **`instance_type`**: EC2 instance type (e.g., `t3.small`).
- **`region`**: AWS region where the resources will be created (e.g., `ap-south-1`).
- **`key_name`**: Name of the SSH key pair used to access the EC2 instance.
- **`volume_size`**: Size of the EBS volume to be attached to the EC2 instance.
- **`ami_os`**: Operating System for the EC2 instance (e.g., `RHEL8`).
- **`survey_teardown_response`**: Determines whether resources should be deleted (`yes` or `no`).
- **`tag_name`**: A base tag name applied to all resources for easy identification.

### Fixed Variables

The following variables are defined within the playbook and are not expected to change dynamically:

- **`vpc_cidr`**: CIDR block for the VPC. (Default: `10.0.0.0/16`)
- **`subnet_cidr`**: CIDR block for the subnet. (Default: `10.0.1.0/24`)

### Vars Files

- **`mapping.yml`**: This file should include mappings for AMI IDs based on the region and OS, as well as the teardown boolean mapping based on the survey response.

## Role Tasks

### 1. **Validation**

- **Fail if `volume_size` is greater than 40GB**: Ensures that the EBS volume size provided by the survey does not exceed 40GB.

### 2. **Resource Creation**

- **VPC Creation**: Checks for an existing VPC by tag name and creates one if it doesn't exist.
- **Subnet Creation**: Creates a subnet within the VPC.
- **Internet Gateway (IGW)**: Creates and attaches an IGW to the VPC.
- **Route Table Management**: Ensures the main route table has a route to the IGW.
- **Security Group**: Creates a security group allowing SSH (22), HTTP (80), and HTTPS (443) traffic.
- **EC2 Instance**: Launches an EC2 instance with the specified AMI, instance type, and attached EBS volume.
- **EBS Volume Attachment**: Attaches an additional EBS volume to the EC2 instance.

### 3. **Resource Teardown**

- **Terminate EC2 Instance**: Terminates the EC2 instance if teardown is requested.
- **Delete Security Group**: Deletes the security group created earlier.
- **Delete Subnet and VPC**: Deletes the subnet and VPC if teardown is requested.

### 4. **Output**

- **Instance Details**: After the instance is created, its details such as Instance ID, Public IP, Private IP, Availability Zone, Instance Type, Key Name, Launch Time, and Security Groups are printed for reference.

## Role Usage

To use this role in your playbook, include it as follows:

```yaml
---
- hosts: localhost
  roles:
    - role: aws.aws_operations.run
