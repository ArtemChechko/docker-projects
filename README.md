![Architecture diagram](architecture diagram.jpg)

---
# Static Website Deployment on AWS using Docker and ECS

This project demonstrates the deployment of a static website on AWS, utilizing Docker and Amazon ECS. The deployment is designed to be highly available, fault-tolerant, and scalable by leveraging various AWS services.

## Prerequisites

### General Prerequisites
1. **Install Git**: [Download Git](https://git-scm.com/downloads) and install it on your computer.
2. **Install Visual Studio Code**: [Download Visual Studio Code](https://code.visualstudio.com/download) and install it.
3. **Register for a GitHub account**: [Sign up for GitHub](https://github.com/join).

### Docker Prerequisites
1. **Create a Key Pair** and add the Public SSH Key to your GitHub account.
2. **Sign up for Docker Hub**: [Docker Hub](https://hub.docker.com/signup).
3. **Download and Install Docker**: [Docker Desktop](https://www.docker.com/products/docker-desktop).

### AWS Prerequisites
1. **Install AWS CLI**: [AWS CLI Installation](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html).
2. **Create IAM User and Named Profile**: Follow [this guide](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_users_create.html) to create an IAM user and [configure your profile](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-profiles.html).

## Part 1: Setting up Docker

1. **Create a GitHub Repository** to store your Dockerfile.
2. **Clone the GitHub Repository** on your computer:
    ```bash
    git clone <your-repo-url>
    ```
3. **Create Dockerfile**: Define the Dockerfile for your static website.
4. **Build the Container Image**:
    ```bash
    docker build -t <image-name> .
    ```
5. **Create a Repository in Docker Hub** to store your image.
6. **Push the Image to Docker Hub**:
    ```bash
    docker tag <image-name> <dockerhub-username>/<repository-name>:<tag>
    docker push <dockerhub-username>/<repository-name>:<tag>
    ```

## Part 2: Deploying on AWS

1. **Create an Amazon ECR Repository** to store your image:
    ```bash
    aws ecr create-repository --repository-name <repository-name>
    ```
2. **Push the Image to ECR**:
    Follow the instructions in the [AWS CLI documentation](https://docs.aws.amazon.com/cli/latest/reference/ecr/get-login-password.html):
    ```bash
    aws ecr get-login-password --region <your-region> | docker login --username AWS --password-stdin <aws-account-id>.dkr.ecr.<your-region>.amazonaws.com
    docker tag <image-name> <aws-account-id>.dkr.ecr.<your-region>.amazonaws.com/<repository-name>:<tag>
    docker push <aws-account-id>.dkr.ecr.<your-region>.amazonaws.com/<repository-name>:<tag>
    ```

## Part 3: AWS Infrastructure Setup

1. **VPC Setup**: Create a VPC with public and private subnets in 2 availability zones.
2. **Internet Gateway**: Allow communication between instances in VPC and the internet.
3. **Security Groups**: Create security groups to act as a firewall for controlling traffic to and from the instances.
4. **High Availability**: Use 2 Availability Zones for high availability and fault tolerance.
5. **Public Subnets**: Resources such as NAT Gateway, Bastion Host, and ALB use public subnets.
6. **Private Subnets**: Place the containers (AWS Fargate) in private subnets for protection.
7. **NAT Gateway**: Allow instances in the private subnets to access the internet.
8. **Application Load Balancer**: Distribute web traffic across an Auto Scaling group of Fargate instances in multiple AZs.
9. **Auto Scaling Group**: Dynamically create Fargate instances to ensure the website is highly available, scalable, fault-tolerant, and elastic.
10. **GitHub**: Store web files in GitHub.
11. **ECS Cluster, Task Definition, and Service**: Create these in the AWS Management Console.
12. **Route 53**: Register your domain name and create a record set pointing to your ALB.
13. **AWS Certificate Manager**: Secure web communications to your website.

## Repository Contents

- **Dockerfile**: Defines the environment for your static website.
- **Reference Diagram**: Shows the architecture and deployment process.

## Conclusion

By following the steps outlined in this guide, you will have successfully deployed a static website on AWS using Docker and Amazon ECS. For more details, refer to the files in this repository.

---
