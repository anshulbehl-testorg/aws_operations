# Overview

## Purpose

The `aws.aws_operations.run` role is designed to automate the management of AWS infrastructure. It allows users to easily create, manage, and teardown key AWS resources such as VPCs, subnets, Internet Gateways (IGWs), Security Groups, and EC2 instances. The role is built to be flexible, with most variables provided through survey inputs, making it adaptable to various environments and use cases.

## Features

- **VPC Management**: Create and manage Virtual Private Clouds (VPCs) with custom CIDR blocks.
- **EC2 Instance Deployment**: Launch EC2 instances with specific instance types, AMI IDs, and attached volumes.
- **Security Management**: Configure Security Groups to allow or restrict access to instances.
- **Teardown Capability**: Easily remove all created resources with a simple teardown command.
- **Dynamic Inputs**: Utilize surveys in AWX/Ansible Tower to dynamically input variables for different runs.

## Architecture

The role follows a modular approach, with tasks structured to check for existing resources before creating new ones. This ensures idempotency, preventing unnecessary resource creation or duplication. The role leverages Ansible's `amazon.aws` collection to interact with AWS services.
