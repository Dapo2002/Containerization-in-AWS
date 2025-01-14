Amazon ECS Deployment Project
Deploy a Web Application to Amazon ECS with EC2, Docker, ECR, Load balancer

This project demonstrates how to deploy a containerized application to Amazon ECS using EC2, Docker, ECR, Fargate, and an Application Load Balancer. Below is a step-by-step outline of the workflow.

Prerequisites
1. AWS Account: Ensure you have an active AWS account.
2. AWS CLI: Installed and configured with access keys.
3. IAM Roles and Permissions: Ensure necessary permissions for EC2, ECS, ECR, and Load Balancer services.
4. Web Application Code: The application to be containerized.


Steps to Deploy the Application

1. Launch an EC2 Instance and Install Docker
- Launch an EC2 instance using AWS Management Console or AWS CLI.
- SSH into the instance and install Docker:
  ```bash
  sudo yum update -y
  sudo yum install docker -y
  sudo service docker start
  ```
- Verify Docker installation:
  ```bash
  docker --version
  ```

 2. Build Docker Image and Containerize the Application
- Navigate to your application directory and create a Dockerfile.
- Build the Docker image:
  ```bash
  docker build -t <image-name> .
  ```
- Run the container locally for testing:
  ```bash
  docker run -p 80:80 <container-name><image-name>
  ```

 3. Create a Repository in Amazon Elastic Container Registry (ECR)
- Use the AWS Management Console or CLI to create an ECR repository:
  ```bash
  aws ecr create-repository --repository-name <repository-name>
  ```
- Note the repository URI for later use.

 4. Push Docker Image to ECR
- Authenticate to ECR:
  ```bash
  aws ecr get-login-password --region <region> | docker login --username AWS --password-stdin <repository-uri>
  ```
- Tag the image:
  ```bash
  docker tag <image-name>:<tag> <repository-uri>:<tag>
  ```
- Push the image to ECR:
  ```bash
  docker push <repository-uri>:<tag>
  ```

 5. Set Up Application Load Balancer
- Create an Application Load Balancer (ALB) in AWS Management Console.
- Configure target groups and health checks.

 6. Deploy the Application Using Amazon ECS
- Create a task definition specifying the container details and ECR image.
- Create an ECS cluster and service.
- Attach the Application Load Balancer to the ECS service.

 7. Validate the Deployment
- Obtain the ALB DNS name from the AWS Management Console.
- Access the application via the DNS name to confirm the deployment is successful.

---

 Outputs
- ECR Repository URI: `<repository-uri>`
- Load Balancer DNS Name: `<alb-dns-name>`
- Cluster Details: ECS Cluster Name, Task Definition, and Service Name.

---

 Cleanup
- Delete ECS services, task definitions, and clusters.
- Deregister targets and delete the Application Load Balancer.
- Delete the ECR repository if no longer needed.

---

 Useful Commands
- List ECS Clusters:
  ```bash
  aws ecs list-clusters
  ```
- Describe Task Definition:
  ```bash
  aws ecs describe-task-definition --task-definition <task-def-name>
  ```
- Delete ECR Repository:
  ```bash
  aws ecr delete-repository --repository-name <repository-name> --force
  
