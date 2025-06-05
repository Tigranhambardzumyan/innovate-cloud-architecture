# 🌐 Network Design – Innovate Inc.

## Overview

To support a secure, scalable, and highly available cloud infrastructure, Innovate Inc. employs a well-structured **Virtual Private Cloud (VPC)** design. The architecture adheres to AWS best practices, ensuring robust network segmentation, least-privilege access, and future scalability.

---

## 🔷 VPC Architecture

Each environment (Development, Staging, Production) resides in its own isolated AWS account and contains a dedicated VPC.

### VPC Configuration (per environment):

| Component              | Description                                                                 |
|------------------------|-----------------------------------------------------------------------------|
| CIDR Block             | `/16` block (e.g., `10.0.0.0/16`)                                           |
| Availability Zones     | Spread across 2–3 AZs for high availability                                 |
| Public Subnets         | `/24` subnets in each AZ for ALBs, NAT Gateways                             |
| Private Subnets        | `/24` subnets in each AZ for app pods, databases, internal services         |
| Isolated Subnets       | Optional, for backend services like RDS not needing internet access         |
| NAT Gateways           | 1 per AZ, enabling outbound access from private subnets                     |
| Route Tables           | Custom per subnet type (public, private, isolated)                          |
| VPC Endpoints          | S3, DynamoDB, and optionally others for private service access              |

---

## 🔐 Network Security

Security is enforced using multiple layered controls:

- **Security Groups**: Applied at EC2/EKS pod level, only allowing required traffic.
- **Network ACLs (NACLs)**: Stateless subnet-level rules as an extra layer.
- **VPC Flow Logs**: Enabled for monitoring and forensic analysis.
- **Subnets Separation**: Public-facing components (e.g., ALBs) are strictly isolated from backend resources.
- **Ingress/Egress Controls**: Only whitelisted ports (e.g., 443 for HTTPS) are opened externally.

---

## ☁️ Application Load Balancing

- **ALB (Application Load Balancer)** is placed in the public subnet.
- It routes traffic to the Kubernetes Ingress Controller deployed in private subnets.
- SSL termination is handled at the ALB using **AWS Certificate Manager (ACM)**.

---

## 🧩 DNS & Service Discovery

- **Route 53** is used for DNS management.
- Internal services may leverage **CoreDNS** inside EKS or **AWS Cloud Map** if needed.

---

## 🔄 High Availability & Scalability

- **Multi-AZ Subnet Design** ensures resilience against AZ failures.
- **Kubernetes nodes** and **RDS instances** are spread across zones.
- **Auto Scaling** (both EC2 and Karpenter) ensures dynamic capacity allocation.

---

## 🔧 Future Enhancements

- **Transit Gateway**: For centralized routing if more accounts or VPCs are added later.
- **Service Mesh (e.g., AWS App Mesh)**: For fine-grained service communication control.
- **PrivateLink**: For secure connections to third-party services or internal APIs.

---

## ✅ Summary

The proposed network design enables:

- ✅ Strong isolation between tiers and environments  
- ✅ High availability across multiple AZs  
- ✅ Secure ingress/egress control and encrypted traffic flow  
- ✅ Scalability and flexibility for future growth  

This architecture lays the foundation for a secure and reliable cloud-native application infrastructure for Innovate Inc.

