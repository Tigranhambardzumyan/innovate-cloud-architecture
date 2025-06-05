# 💰 Innovate Inc. – Cloud Infrastructure Cost Estimation

---

## 📦 Purpose

This document provides a high-level monthly cost estimate for Innovate Inc.'s AWS infrastructure and integrated third-party tools, based on the designed architecture.

---

## 🧱 AWS Infrastructure Cost Summary

| Component             | Monthly Cost (USD) | Description                                                      |
|----------------------|--------------------|------------------------------------------------------------------|
| Amazon EKS           | $899.25            | 3 clusters + mixed instance types (prod, staging, dev)          |
| Amazon RDS           | $40.37             | PostgreSQL + 100 GB storage                                     |
| AWS Client VPN       | $401.50            | 3 subnet associations + 5 concurrent users                      |
| AWS Transit Gateway  | $119.50            | Attachments + 500 GB of data processing                         |
| Amazon CloudWatch    | $101.30            | Metrics, logs ingestion, and retention                         |
| AWS CloudTrail       | $1.00              | 1M API events                                                   |
| **Total AWS Infra**  | **$1,682.92**      |                                                                  |

---

## 🔗 Third-Party & Open Source Tooling

| Tool         | Monthly Cost (USD) | Description                                                      |
|--------------|--------------------|------------------------------------------------------------------|
| Slack        | $145.00            | Slack Pro plan for 20 users                                     |
| Grafana      | $30–50             | Self-hosted on EC2/EKS                                          |
| OpenVPN      | $0                 | Handled via AWS Client VPN                                      |
| Prometheus   | $25–40             | Self-hosted on EKS, includes compute and storage                |
| Okta SSO     | $120–160           | Business SSO tier for 20 users                                  |
| **Total 3rd-Party** | **$320–395**     |                                                                  |

---

## 📊 Combined Monthly Estimate

| Category         | Estimated Monthly Cost |
|------------------|------------------------|
| AWS Infrastructure | $1,682.92             |
| 3rd-Party & Monitoring | $320–395           |
| **Total**        | **$2,002.92 – $2,077.92** |

---

## 📌 Notes

- All costs are based on **us-east-1 (N. Virginia)** pricing as of 2025.
- AWS costs assume 730 hours/month per instance.
- Spot instance pricing is estimated based on current market rates.
- Estimates are subject to change with actual usage, reserved instances, or AWS pricing updates.

For detailed planning, please refer to the [AWS Pricing Calculator](https://calculator.aws.amazon.com/).

