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

