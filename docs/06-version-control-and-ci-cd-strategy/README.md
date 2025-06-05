# ✅ Innovate Inc. – Version Control and CI/CD Strategy(GitHub Actions + OIDC)

---

## 1. Version Control: Why GitHub?

GitHub is selected as the version control and collaboration platform for **Innovate Inc.** due to its mature DevOps ecosystem, seamless CI/CD integrations, and strong security model.

### ✅ Key Reasons for Choosing GitHub

- Industry-standard Git platform with developer familiarity
- Native support for **GitHub Actions** for CI/CD automation
- **OIDC integration** for secure, short-lived AWS credentials (no long-term secrets)
- Granular access control using GitHub organizations, teams, and roles
- Pull request workflows for structured code review
- Built-in tools: issues, boards, milestones, security alerts
- Rich ecosystem of integrations (Slack, Jira, Datadog, etc.)
- Automated vulnerability scanning and secret detection

By consolidating **source control**, **CI/CD**, and **collaboration** in GitHub, Innovate Inc. achieves a scalable and secure development workflow fully aligned with cloud-native best practices.

---

## ✅ Recommended Runner Type: Self-Hosted GitHub Actions Runner in AWS

GitHub Actions integrates with AWS using **OIDC**, allowing Innovate Inc. to run secure deployment pipelines without long-lived secrets.

### Runner Deployment Strategy

- The **self-hosted runner** is deployed in EC2 within the **`innovate-dev`** VPC.
- It assumes IAM roles in **staging** and **production** accounts using **OIDC** and **cross-account trust**.
- The runner has **no public IP** and runs in a **private subnet** with NAT access.
- CI/CD pipelines promote code between environments: **Dev → Staging → Prod**, with approval gates.

---

### ⚙️ Recommended Runner Configuration

| **Component**      | **Recommendation**                              |
|--------------------|--------------------------------------------------|
| Environment         | EC2 instance (e.g., `t3.medium`) in `innovate-dev` |
| OS                  | Ubuntu 22.04 LTS                                 |
| Installation        | Manual or via Terraform                          |
| IAM Access          | OIDC-trusted role per environment                |
| Network             | Private subnet + NAT gateway                     |

---

## ✅ Benefits of Self-Hosted Runners

| **Feature**            | **Benefit to Innovate Inc.**                        |
|------------------------|-----------------------------------------------------|
| **Performance**        | Faster builds, no cold starts                       |
| **Security**           | Runs in VPC with no public IP                       |
| **Internal Access**    | Access to VPC-only resources (EKS, RDS, S3)         |
| **Cost Control**       | Predictable EC2 pricing vs. GitHub-hosted billing   |
| **Credential Mgmt**    | Secure OIDC-based access (no static AWS keys)       |

---

## 🛠️ When to Use GitHub-Hosted Runners

Use GitHub-hosted runners when:

- Working on public or open-source repos
- Running low-frequency CI pipelines
- No AWS integration is required
- You prefer zero maintenance

---

## 🔐 OIDC Access Model

Each target AWS account (Dev, Staging, Prod) includes:

- An **IAM role** with an OIDC trust policy for GitHub
- Fine-grained permissions (e.g., deploy to EKS, push to S3, etc.)
- GitHub Actions assume this role securely for deployments
