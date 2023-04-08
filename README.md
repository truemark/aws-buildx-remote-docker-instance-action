# AWS Remote Docker Instance Action

[![LICENSE](https://img.shields.io/badge/license-BSD3-green)](LICENSE)
[![Latest Release](https://img.shields.io/github/v/release/truemark/aws-buildx-remote-docker-instance-action)](https://github.com/truemark/aws-buildx-remote-docker-instance-action/releases)
![GitHub closed issues](https://img.shields.io/github/issues-closed/truemark/aws-buildx-remote-docker-instance-action)
![GitHub closed pull requests](https://img.shields.io/github/issues-pr-closed/truemark/aws-buildx-remote-docker-instance-action)

[//]: # (![build-test]&#40;https://github.com/truemark/aws-buildx-remote-docker-instance-action/workflows/build-test/badge.svg&#41;)

Creates one arm64 and one amd64 ephemeral EC2 instances running a docker daemon for use by tools like docker/buildx to improve build times for multi-architecture image builds 

## Examples

```yml
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v3
      - name: Configure AWS Credentials
        uses: aws-actions/configure-aws-credentials@v2
        with:
          role-to-assume: "${{ secrets.AWS_ASSUME_ROLE }}"
          aws-region: "us-east-2"
      - name: Login to Docker Hub
        uses: docker/login-action@v2
        with:
          username: ${{ secrets.DOCKER_HUB_USERNAME }}
          password: ${{ secrets.DOCKER_HUB_ACCESS_TOKEN }}
      - name: Setup Buildx Remote Docker
        id: buildx
        uses: truemark/aws-buildx-remote-docker-instance-action@v1
        with:
          security-group-id: "sg-0baf5bcfe9f21efaa"
          subnet-id: "subnet-09a35a2abd797dbfa"
          instance-profile: "TruemarkEC2RoleforSSM"
          region: "us-east-2"
      - name: Build images
        run: |
          docker buildx build \
            --push \
            --platform linux/arm64,linux/amd64 \
            -f amazonlinux.Dockerfile \
            -t truemark/aws-cdk:beta-amazonlinux \
            -t truemark/aws-cdk:beta \
            .
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

## Outputs
| Name              | Type       | Description                           |
|-------------------|------------|---------------------------------------|
| arm64-instance-id | string     | Instance ID of the arm64 EC2 instance |
| amd64-instance-id | string     | Instance ID of the amd64 EC2 instance |
