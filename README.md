\# AWS Terraform Container Infrastructure



\## Project Overview



This project demonstrates the deployment of a containerized web application on AWS using Infrastructure as Code (IaC).



The infrastructure is being built with Terraform and includes a custom VPC, public and private subnets across multiple Availability Zones, route tables, security groups, an Application Load Balancer architecture, Amazon ECR, and Amazon ECS with AWS Fargate.



The application is containerized using Docker and stored in Amazon Elastic Container Registry (ECR).



\## Project Goals



\- Build AWS infrastructure using Terraform instead of manually creating resources in the AWS Console.

\- Deploy infrastructure across multiple Availability Zones.

\- Containerize a web application using Docker.

\- Store Docker images in Amazon ECR.

\- Deploy containers using Amazon ECS with AWS Fargate.

\- Route application traffic through an Application Load Balancer.

\- Implement monitoring using Amazon CloudWatch.

\- Implement a CI/CD workflow for automated application deployment.



\## Technologies Used



\- AWS

\- Terraform

\- Docker

\- Amazon ECR

\- Amazon ECS

\- AWS Fargate

\- Application Load Balancer

\- Amazon CloudWatch

\- Git

\- GitHub

\- PowerShell



\## Current Project Status



Completed:



\- Terraform project initialization

\- Custom VPC

\- Two public subnets across two Availability Zones

\- Two private subnets across two Availability Zones

\- Internet Gateway

\- Public and private route tables

\- Route table associations

\- Application Load Balancer security group

\- Application security group

\- Application Load Balancer target group

\- Amazon ECR repository

\- Docker application files

\- Docker image built and tested locally

\- Docker image pushed to Amazon ECR

\- Git repository initialized

\- Project source code pushed to GitHub



In Progress:



\- ECS cluster and Fargate deployment

\- Application Load Balancer deployment

\- CloudWatch logging and monitoring

\- CI/CD pipeline



\## Architecture



The application is designed using a multi-AZ AWS architecture.



\### Application Traffic Flow



Internet User  

↓  

Application Load Balancer  

↓  

HTTP Listener (Port 80)  

↓  

Target Group  

↓  

ECS Fargate Task  

↓  

Docker Container  

↓  

Nginx Web Application



\### Container Deployment Flow



Application Files  

↓  

Docker Image  

↓  

Amazon ECR  

↓  

Amazon ECS  

↓  

AWS Fargate  

↓  

Running Container



\### Network Architecture



\- VPC CIDR: `10.0.0.0/16`

\- Public Subnet 1: `10.0.1.0/24` - `us-east-1a`

\- Public Subnet 2: `10.0.2.0/24` - `us-east-1b`

\- Private Subnet 1: `10.0.3.0/24` - `us-east-1a`

\- Private Subnet 2: `10.0.4.0/24` - `us-east-1b`

\- Application Load Balancer designed for the public subnets

\- ECS Fargate tasks designed for the application tier

\- Application security group accepts HTTP traffic only from the ALB security group



\## Docker and Amazon ECR



The web application was containerized using Docker with an Nginx Alpine base image.



The Docker image was built locally and tested before being uploaded to Amazon ECR.



\### Docker Build



The image was built using:



```bash

docker build -t terraform-project-app .

```

\## ECR Authentication Troubleshooting



While pushing the Docker image to Amazon ECR, I encountered an authentication issue when using the standard AWS-recommended Docker login command:



```powershell

aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin <ECR-REGISTRY>

```



Docker returned:



```text

400 Bad Request

```



\### Troubleshooting Performed



To isolate the issue, I performed several troubleshooting steps:



\- Verified that the AWS CLI was installed and working.

\- Verified the authenticated AWS identity.

\- Confirmed that the correct AWS region and ECR registry were being used.

\- Confirmed that AWS was successfully generating an ECR authentication password.

\- Verified that Docker Desktop and the Docker daemon were running.

\- Verified the active Docker context.

\- Tested authentication using the default AWS CLI profile.

\- Tested Docker authentication with a clean Docker configuration.

\- Confirmed that the ECR authentication credentials themselves were valid.



The AWS ECR password was being generated successfully, but Docker authentication continued to return `400 Bad Request` when the password was passed through `--password-stdin` in my Windows PowerShell environment.



\### Workaround



I stored the ECR password in a PowerShell variable:



```powershell

$password = aws ecr get-login-password --region us-east-1

```



I then authenticated Docker using the generated password directly:



```powershell

docker login --username AWS --password "$password" <ECR-REGISTRY>

```



Docker successfully authenticated:



```text

Login Succeeded

```



> \*\*Security Note:\*\* `--password-stdin` is the preferred authentication method because passing credentials directly on the command line can expose them through command history or process information. The direct password method was used only as a troubleshooting workaround.



After successful authentication, I tagged the local Docker image with the ECR repository URI and pushed it to Amazon ECR:



```powershell

docker tag terraform-project-app:latest <ECR-REPOSITORY-URI>:latest

docker push <ECR-REPOSITORY-URI>:latest

```



The Docker image was successfully uploaded to Amazon ECR and is ready to be used by Amazon ECS.

