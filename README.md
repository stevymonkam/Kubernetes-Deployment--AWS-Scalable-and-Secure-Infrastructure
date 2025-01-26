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

### 1. **AWS WAF (Web Application Firewall)**

- Protège l'application contre les exploits Web courants et les bots.
- Configuré avec des règles personnalisées pour garantir que seules les requêtes valides sont transmises à CloudFront.

### 2. **AWS CloudFront**

- Un réseau de diffusion de contenu (CDN) qui offre une faible latence.
- Distribue le trafic vers le système backend via le domaine `cf.domain.com`.

### 3. **AWS Classic Load Balancer**

- Gère le trafic provenant de CloudFront et le distribue à l'Ingress Kubernetes.
- Exposé via le domaine `lb.domain.com`.

### 4. **Cluster Kubernetes**

- Héberge les applications conteneurisées.
- Utilise un Ingress Kubernetes pour acheminer le trafic vers les services appropriés dans le cluster.

### 5. **AWS Route 53**

- Fournit le routage DNS pour `cf.domain.com` et `lb.domain.com`.
- Configuré avec external-dns pour gérer automatiquement les enregistrements DNS des services Kubernetes.

---

## Prérequis

Pour déployer ce projet, assurez-vous de disposer des outils et ressources suivants :

1. **Compte AWS** : Un compte AWS actif avec des autorisations pour créer des ressources comme WAF, CloudFront, Load Balancer, etc.
2. **Cluster Kubernetes** :
   - Provisionné avec EKS, Kops ou tout autre outil de déploiement Kubernetes.
   - Les nœuds doivent être déployés dans des sous-réseaux privés pour plus de sécurité.
3. **AWS CLI** :
   - Installé et configuré avec un accès à votre compte AWS.
4. **kubectl** :
   - Installé et configuré pour gérer votre cluster Kubernetes.
5. **Helm** :
   - Installé pour déployer les ressources Kubernetes.

---

## Étapes de déploiement

### 1. **Provisionner le cluster Kubernetes**

- Déployez un cluster Kubernetes dans des sous-réseaux privés.
- Assurez-vous que le cluster a accès aux services AWS comme Route 53 et le Load Balancer.

### 2. **Configurer le Load Balancer**

- Créez un AWS Classic Load Balancer dans le sous-réseau public.
- Configurez le Load Balancer pour transmettre le trafic à l'Ingress Kubernetes.

### 3. **Déployer les ressources Kubernetes**

- **Ingress Controller** : Déployez un contrôleur NGINX ou AWS ALB Ingress pour gérer le routage HTTP/S dans le cluster.
  ```bash
  helm install nginx-ingress ingress-nginx/ingress-nginx
  ```
- **External-DNS** : Déployez external-dns pour gérer dynamiquement les enregistrements DNS dans Route 53.
  ```bash
  helm install external-dns bitnami/external-dns \
      --set provider=aws \
      --set aws.region=<AWS_REGION>
  ```

### 4. **Configurer Route 53**

- Créez des enregistrements Route 53 pour `cf.domain.com` et `lb.domain.com` pointant respectivement vers CloudFront et le Load Balancer.
- Assurez-vous qu'external-dns dispose des autorisations IAM pour mettre à jour les enregistrements DNS.

### 5. **Configurer AWS CloudFront**

- Configurez une distribution CloudFront pour le domaine `cf.domain.com`.
- Définissez le Load Balancer comme origine pour CloudFront.

### 6. **Configurer AWS WAF**

- Associez le WAF à CloudFront pour filtrer les requêtes entrantes.
- Définissez des règles pour bloquer le trafic indésirable.

---

## Tests

1. **Accéder à l'application** :
   - Accédez à `cf.domain.com` pour tester l'application via CloudFront.
   - Utilisez `lb.domain.com` pour tester directement le Load Balancer.
2. **Valider le DNS** :
   - Vérifiez que les enregistrements DNS sont créés et correctement pointés via Route 53.
   - Utilisez des outils comme `dig` ou `nslookup` pour valider.
3. **Surveiller le trafic** :
   - Utilisez AWS CloudWatch et les journaux WAF pour surveiller le trafic entrant.
![image](https://github.com/user-attachments/assets/541bbaca-ce7e-4708-a7c2-79893044ec42)


