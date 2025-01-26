# Kubernetes-Deployment--AWS-Scalable-and-Secure-Infrastructure

This project presents a complete infrastructure to deploy applications on a Kubernetes cluster hosted on AWS. The stack has been deployed with CloudFormation. The architecture is designed for high availability, strong security, and scalability thanks to AWS services like CloudFront, WAF,ACM, Route 53,internet gateway,ALB,external-dns,vpc.subnet,Ec2,bastion, and a Kubernetes Ingress.




| **AWS EKS DEPLOY WITH CLOUDFORMATION**      | **high availability, scalability and security (HTTPS AND CERTIFICATE) with (DNS, ACM, INGRESS, ROUTE 53, EXTERNAL-DNS)**     |
|------------------|-------------|
| ![](aws-eks.PNG) | ![](dns.PNG) |




Cette stack CloudFormation déploie une instance EC2 préconfigurée pour créer un cluster EKS (Kubernetes) sur AWS. Elle inclut :

Ressources IAM : Crée un rôle et un profil d'instance avec des permissions pour gérer le cluster.
Instance EC2 : Déploie une instance avec tous les outils nécessaires (kubectl, AWS CLI, etc.) et un script d'amorçage pour lancer le cluster EKS.
Groupe de sécurité : Autorise les connexions SSH (port 22) et HTTP (port 80).
Déploiement automatique d'EKS : Télécharge un modèle EKS et lance un cluster dans deux sous-réseaux publics.
Enfin, des outputs affichent l'ID, l'adresse publique et la zone de disponibilité de l'instance EC2.

