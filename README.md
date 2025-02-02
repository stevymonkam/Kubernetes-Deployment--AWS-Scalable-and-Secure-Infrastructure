# Kubernetes-Deployment--AWS-Scalable-and-Secure-Infrastructure

This project presents a complete infrastructure to deploy applications on a Kubernetes cluster hosted on AWS. The stack has been deployed with CloudFormation. The architecture is designed for high availability, strong security, and scalability thanks to AWS services like CloudFront, WAF,ACM, Route 53,internet gateway,ALB,external-dns,vpc.subnet,Ec2,bastion, and a Kubernetes Ingress.




| **AWS EKS DEPLOY WITH CLOUDFORMATION**      | **high availability, scalability and security (HTTPS AND CERTIFICATE) with (DNS, ACM, INGRESS, ROUTE 53, EXTERNAL-DNS)**     |
|------------------|-------------|
| ![](aws-eks.PNG) | ![](dns.PNG) |





### AWS EKS DEPLOY WITH CLOUDFORMATION

This CloudFormation stack deploys a pre-configured EC2 instance to create an EKS (Kubernetes) cluster on AWS. It includes:

IAM Resources: Creates a role and instance profile with permissions to manage the cluster.
EC2 Instance: Deploys an instance with all the necessary tools (kubectl, AWS CLI, etc.) and a bootstrap script to launch the EKS cluster.
Security Group: Allows SSH (port 22) and HTTP (port 80) connections.
Automated EKS Deployment: Downloads an EKS template and launches a cluster in two public subnets.
Finally, outputs display the EC2 instance ID, public address, and availability zone.

## [code stack cloudformation](aws-eks.yaml)






###  high availability, scalability and security (HTTPS AND CERTIFICATE) with (DNS, ACM, INGRESS, ROUTE 53, EXTERNAL-DNS)
## Composants et flux de travail




   

