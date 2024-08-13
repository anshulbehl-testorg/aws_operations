# Setup Guide

## Prerequisites

Before using the `aws.aws_operations.run` role, ensure you have the following:

- **Ansible**: Installed on your control node (version 2.9+ recommended).
- **AWS Credentials**: Configured on your control node or in the environment where the role will run.
- **Required Collections**: Install the necessary Ansible collections:
  
  ```bash
  ansible-galaxy collection install amazon.aws
