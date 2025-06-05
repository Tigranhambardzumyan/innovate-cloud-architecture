# 🛠️ Infrastructure as Code – AWS CloudFormation with SAM & GitHub Actions

---

## ✅ Purpose

Innovate Inc. uses **AWS CloudFormation** and **AWS SAM** to manage infrastructure through **Infrastructure-as-Code (IaC)**. This ensures environments are consistent, reproducible, and auditable, and deployments are automated using **GitHub Actions**.

---


## 🤔 Why Not Terraform? Why Choose CloudFormation + SAM?

While **Terraform** is a powerful and widely adopted Infrastructure-as-Code (IaC) tool, Innovate Inc. has chosen to standardize on **AWS CloudFormation** combined with **AWS SAM (Serverless Application Model)** for the following reasons:

### ✅ 1. Native AWS Integration
- CloudFormation and SAM are AWS-native tools with first-class support for all AWS services and features.
- No need for third-party providers or waiting for support in Terraform’s AWS plugin.

### ✅ 2. SAM for Serverless Workloads
- SAM simplifies deployment of Lambda functions, API Gateway, and event-driven architectures.
- Provides built-in support for local development, testing, and deploying serverless applications.

### ✅ 3. GitHub Actions + OIDC = Secure CI/CD
- AWS offers first-party GitHub Actions with secure, short-lived OIDC credentials.
- Avoids hardcoded AWS credentials in CI pipelines.

### ✅ 4. Change Sets and Safe Deployments
- CloudFormation supports Change Sets to preview changes before deployment.
- Rollback mechanisms protect production environments from misconfigurations.

### ✅ 5. Governance with StackSets and Service Catalog
- CloudFormation integrates directly with AWS Organizations and StackSets for multi-account governance.
- Enables centralized policies, logging, and tagging enforcement.

### ✅ 6. Lower Complexity for Mixed Workloads
- No need to mix Terraform with SAM for serverless — SAM and CloudFormation handle both infrastructure and functions natively.

---



## 🧱 Key Infrastructure Components Managed

| Resource Category      | Description                                                                 |
|------------------------|-----------------------------------------------------------------------------|
| **VPCs & Subnets**     | Full network architecture, including public/private subnets, NACLs, and routing |
| **EKS Clusters**       | Kubernetes clusters and node groups with autoscaling and role-based access  |
| **IAM Roles & Policies**| IAM entities for EC2, EKS, and GitHub OIDC-based deployments                |
| **Monitoring & Logging**| VPC flow logs, CloudWatch, EKS audit logs, GuardDuty, and alerts            |
| **Resource Tagging**   | Tags enforced for cost, ownership, environment, and compliance               |

---

## 🧩 CloudFormation Strategy

- **Modular Templates**  
  Each infrastructure component (e.g., VPC, EKS, IAM, Monitoring) is defined in a **reusable YAML template**, promoting clarity and reusability.

- **Environment Isolation**  
  Separate CloudFormation stacks per environment (`Dev`, `Staging`, `Prod`) for safe isolation and targeted updates.

- **CloudFormation StackSets**  
  Used for shared governance across accounts:
  - SCPs (Service Control Policies)
  - Centralized logging (CloudTrail, S3, AWS Config)
  - AWS Config rules, compliance baselines

---

## ⚙️ SAM + GitHub Actions Integration

The deployment workflow is automated using **GitHub Actions** and **OpenID Connect (OIDC)**:

- `sam build` compiles the SAM templates
- `sam deploy` updates CloudFormation stacks with change sets
- GitHub OIDC federates secure, short-term AWS credentials — no long-lived secrets

```yaml
name: Deploy CloudFormation with SAM

on:
  push:
    branches: [ main ]

jobs:
  deploy:
    runs-on: ubuntu-latest
    permissions:
      id-token: write
      contents: read
    steps:
      - uses: actions/checkout@v4
      - uses: aws-actions/configure-aws-credentials@v3
        with:
          role-to-assume: arn:aws:iam::123456789012:role/GitHubOIDCDeployRole
          aws-region: us-east-1
      - run: sam build
      - run: sam deploy --stack-name my-app --capabilities CAPABILITY_IAM
```

---

## 🧪 Benefits

| Benefit            | Explanation                                                                 |
|--------------------|-----------------------------------------------------------------------------|
| **Consistency**     | All infra is provisioned from a single, version-controlled source          |
| **Traceability**    | Every change is logged via Git commits and pull requests                   |
| **Safe Updates**    | CloudFormation change sets provide preview before deployment               |
| **Audit Readiness** | Fully track who deployed what, where, and when                             |
| **Zero-Touch Deployments** | Automated GitHub Actions pipeline with secure OIDC integration     |

---

## 📌 Summary

✅ Declarative, versioned infrastructure using **CloudFormation** and **SAM**  
✅ Modular templates for scalability and reuse  
✅ Secure CI/CD with GitHub Actions + OIDC  
✅ Ready for compliance, rollback, and centralized governance  


---

### 📌 Summary

| Reason                          | Benefit                                                   |
|---------------------------------|------------------------------------------------------------|
| AWS-native                      | Fast support for new AWS features                         |
| Built for serverless            | SAM simplifies Lambda, API Gateway, EventBridge, etc.     |
| Secure CI/CD                    | GitHub Actions + OIDC = no secrets rotation               |
| Safe deployments                | Change Sets, rollback, and drift detection                |
| Enterprise governance           | StackSets, tagging, SCPs, AWS Config integration          |
