# ✅ Innovate Inc. – Network Design Strategy

---

## 🌐 VPC Architecture Overview

Each AWS account (`innovate-dev`, `innovate-staging`, `innovate-prod`) will host its own **dedicated VPC**, designed to provide:

- Multi-AZ high availability
- Network isolation between public-facing and internal services
- Scalable private infrastructure for EKS, RDS, and supporting services

---

### 🧱 VPC Design (per environment)

| Component         | Configuration                                                  |
|------------------|------------------------------------------------------------------|
| CIDR Range        | `10.X.0.0/16` (unique per account)                              |
| Availability Zones | 2 or 3 AZs for high availability                               |
| Public Subnets    | 1 per AZ, used for NAT Gateway, Load Balancers                 |
| Private Subnets   | 1 per AZ, used for EKS nodes, RDS, internal services           |
| NAT Gateway       | 1 per AZ for fault-tolerant outbound internet access           |
| Internet Gateway  | Attached to VPC for public subnet outbound connectivity         |
| Route Tables      | Separate per subnet type; tightly controlled routing            |
| DNS               | Enabled for private hostname resolution (AmazonProvidedDNS)     |

---

### 📦 Example CIDR Allocation

| Environment      | VPC CIDR     | AZs         | Public Subnets         | Private Subnets        |
|------------------|--------------|-------------|-------------------------|-------------------------|
| Dev              | 10.10.0.0/16 | 2           | 10.10.1.0/24, 10.10.2.0/24 | 10.10.11.0/24, 10.10.12.0/24 |
| Staging          | 10.20.0.0/16 | 2           | 10.20.1.0/24, 10.20.2.0/24 | 10.20.11.0/24, 10.20.12.0/24 |
| Production       | 10.30.0.0/16 | 3           | 10.30.1.0/24, 10.30.2.0/24, 10.30.3.0/24 | 10.30.11.0/24, 10.30.12.0/24, 10.30.13.0/24 |

---

## 🔐 Network Security Controls

### ✅ Security Groups (SGs)
- **Stateless firewalls** scoped to resources (e.g., EKS, ALB, RDS)
- Minimal open ports, principle of least privilege
- Logging via VPC Flow Logs and CloudTrail

### ✅ Network ACLs (NACLs)
- Enforced at subnet level
- Used for deny rules and broad protection between subnet zones

### ✅ VPC Endpoints (Interface/Gateway)
- Used for private access to AWS services (e.g., S3, ECR, Secrets Manager)
- Avoids routing through the internet for internal data access

---

## 🚦 High Availability & Fault Tolerance

- Multi-AZ deployment ensures continuity during AZ outages
- Subnets are evenly distributed across availability zones
- NAT Gateways deployed per AZ (not shared) to avoid single points of failure

---

## 🔁 Optional: Future Expansion

| Feature                  | Benefit                                                      |
|--------------------------|--------------------------------------------------------------|
| **Transit Gateway**      | Simplifies VPC peering and cross-account routing             |
| **Client VPN**           | Enables secure developer access to private subnets           |
| **WAF + Shield**         | Protects public-facing services from common threats          |
| **Private Hosted Zones** | DNS resolution scoped to VPC without public exposure         |

---

## 📌 Summary

Innovate Inc.'s VPC design provides:

- ✅ Secure, scalable, and highly available infrastructure
- ✅ Clear separation of public and private services
- ✅ Built-in support for managed Kubernetes (EKS), RDS, CI/CD pipelines
- ✅ Foundation for zero-trust security and future network growth
