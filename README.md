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


# Deploy EKS App with Route 53 and SSL on AWS

## Overview
This guide explains how to deploy Angular and React applications on AWS EKS with a Route 53 domain and SSL using AWS Certificate Manager (ACM).

## Table of Contents
- [Prerequisites](#prerequisites)
- [Deploy Angular and React Apps](#deploy-angular-and-react-apps)
- [Configure Load Balancer and SSL](#configure-load-balancer-and-ssl)
- [Create SSL Certificate in AWS ACM](#create-ssl-certificate-in-aws-acm)
- [Configure DNS in Route 53](#configure-dns-in-route-53)
- [Final Steps](#final-steps)

---

## Prerequisites
Before proceeding, ensure you have:
- An AWS account with EKS, ACM, and Route 53 set up
- `kubectl` and `aws-cli` installed and configured
- A registered domain in Route 53

---

## Deploy Angular and React Apps

### Angular App Deployment
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: angular-app
  namespace: default
spec:
  replicas: 3
  selector:
    matchLabels:
      app: angular-app
  template:
    metadata:
      labels:
        app: angular-app
    spec:
      containers:
      - name: angular-app-container
        image: stevymonkam/kuberneteimgfront:1.0
        ports:
        - containerPort: 80
```

### React App Deployment
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: react-app
  namespace: default
spec:
  replicas: 3
  selector:
    matchLabels:
      app: react-app
  template:
    metadata:
      labels:
        app: react-app
    spec:
      containers:
      - name: react-app-container
        image: stevymonkam/contratti-image:1.0
        ports:
        - containerPort: 80
```

---

## Configure Load Balancer and SSL

### Angular App Service
```yaml
apiVersion: v1
kind: Service
metadata:
  name: angular-app-service
  annotations:
    service.beta.kubernetes.io/aws-load-balancer-ssl-cert: "arn:aws:acm:us-east-1:615299770598:certificate/a4806da9-3d96-45f2-a08d-05e380e9ef5e"
    service.beta.kubernetes.io/aws-load-balancer-backend-protocol: "http"
    service.beta.kubernetes.io/aws-load-balancer-ssl-ports: "443"
    service.beta.kubernetes.io/aws-load-balancer-connection-idle-timeout: "3600"
spec:
  selector:
    app: angular-app
  ports:
    - name: http
      port: 80
      targetPort: 80
    - name: https
      port: 443
      targetPort: 80
  type: LoadBalancer
```

### React App Service
```yaml
apiVersion: v1
kind: Service
metadata:
  name: react-service-app
  annotations:
    service.beta.kubernetes.io/aws-load-balancer-ssl-cert: "arn:aws:acm:us-east-1:615299770598:certificate/a4806da9-3d96-45f2-a08d-05e380e9ef5e"
    service.beta.kubernetes.io/aws-load-balancer-backend-protocol: "http"
    service.beta.kubernetes.io/aws-load-balancer-ssl-ports: "443"
    service.beta.kubernetes.io/aws-load-balancer-connection-idle-timeout: "3600"
spec:
  selector:
    app: react-app
  ports:
    - name: http
      port: 80
      targetPort: 80
    - name: https
      port: 443
      targetPort: 80
  type: LoadBalancer
```

---

## Create SSL Certificate in AWS ACM
1. Navigate to **AWS Certificate Manager (ACM)**.
2. Generate a new **public SSL certificate**.
3. Copy the **ARN** of the certificate and replace it in the service annotations above.

To apply the configurations:
```sh
kubectl apply -f .
```

---

## Configure DNS in Route 53
1. Navigate to **AWS Route 53**.
2. Create an **A record** pointing to the Load Balancer.
3. Use an **Alias record** if supported.
4. Verify that the domain resolves correctly.

---

## Final Steps
After completing these steps:
1. Applications are deployed on **EKS**.
2. **Load Balancers** expose them securely with **SSL**.
3. **Route 53** links the domain to the Load Balancer.

Your applications are now secure and publicly accessible over **HTTPS**! 🎉

---

## License
This project is open-source and available under the [MIT License](LICENSE).




   

