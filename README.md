# AWS Remote Docker Instance Action

[![LICENSE](https://img.shields.io/badge/license-BSD3-green)](LICENSE)
[![Latest Release](https://img.shields.io/github/v/release/truemark/aws-ec2-run-instance-action)](https://github.com/truemark/aws-ec2-run-instance-action/releases)
![GitHub closed issues](https://img.shields.io/github/issues-closed/truemark/aws-ec2-run-instance-action)
![GitHub closed pull requests](https://img.shields.io/github/issues-pr-closed/truemark/aws-ec2-run-instance-action)
![build-test](https://github.com/truemark/aws-ec2-run-instance-action/workflows/build-test/badge.svg)

Creates one arm64 and one amd64 ephemeral EC2 instances running a docker daemon for use by tools like docker/buildx to improve build times for multi-architecture image builds 

## Examples

```yml

```

## Inputs

| Name                        | Type       | Required | Description                                     |
|-----------------------------|------------|----------|-------------------------------------------------|
| subnet-id                   | string     | Yes      | Subnet ID to launch the instances in            |
| security-group-id           | string     | Yes      | Security group to apply to the EC2 instances    |
| arm64-instance-type         | string     | Yes      | Instance type to use for the arm64 EC2 instance |
| amd64-instance-type         | string     | Yes      | Instance type to use for the amd64 EC2 instance |
| instance-profile            | string     | No       | Instance profile to use for the EC2 instances   |
| volume-size                 | number     | No       | Volume size in GB to use for the EC2 instances  |
| associate-public-ip-address | boolean    | No       | Associate a public IP address to the instances  |
| tags                        | string     | No       | Tags to apply to the EC2 instance. Format: JSON |
| region                      | string     | Yes      | AWS region to use for the EC2 instances         |
| key-name                    | string     | No       | SSH key name to use for the EC2 instances       |
| ssh-public-key              | string     | No       | SSH public key to use for the EC2 instances     |

## Outputs
| Name              | Type       | Description                           |
|-------------------|------------|---------------------------------------|
| arm64-instance-id | string     | Instance ID of the arm64 EC2 instance |
| amd64-instance-id | string     | Instance ID of the amd64 EC2 instance |
