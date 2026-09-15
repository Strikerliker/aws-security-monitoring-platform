# AWS Security Monitoring Platform

A production-style AWS cloud-security reference implementation for centralized audit logging, threat detection, finding aggregation, alert routing, threat hunting, and incident response.

**Portfolio dashboard:** https://dumm.cloud/projects/aws-security-monitoring-platform/dashboard.html

## What this project demonstrates

- Multi-Region AWS CloudTrail logging with log-file validation
- Centralized, versioned, encrypted S3 log storage with public access blocked
- Amazon GuardDuty threat detection
- AWS Security Hub finding aggregation
- Amazon EventBridge rules for actionable findings
- Encrypted Amazon SNS alert routing
- Terraform-based infrastructure as code
- CloudTrail threat-hunting queries
- Incident-response runbook and architecture documentation
- GitHub Actions Terraform validation
- Live project dashboard backed by public GitHub Actions metadata

## Architecture

```text
AWS Accounts / Workloads
        |
        +----------------------------+
        |                            |
        v                            v
   AWS CloudTrail               Amazon GuardDuty
        |                            |
        v                            v
Encrypted S3 Log Archive       AWS Security Hub
                                     |
                                     v
                              Amazon EventBridge
                               /              \
                              /                \
                     GuardDuty Findings   High/Critical
                              \                /
                               \              /
                                v            v
                              Encrypted SNS Topic
                                      |
                                      v
                              Security Operations
                                      |
                                      v
                               Incident Response
```

## Repository contents

- `terraform/main.tf` — CloudTrail, S3, GuardDuty, Security Hub, EventBridge, and SNS
- `terraform/variables.tf` — project inputs
- `terraform/outputs.tf` — deployment outputs
- `docs/architecture.md` — design decisions and security controls
- `docs/incident-response-runbook.md` — triage and escalation workflow
- `queries/cloudtrail-threat-hunting.sql` — example CloudTrail hunting queries
- `dashboard.html` — project/security operations dashboard
- `.github/workflows/aws-security-monitoring-platform.yml` — Terraform CI validation

## Security controls

- S3 Block Public Access enabled
- S3 server-side encryption enabled
- S3 versioning enabled
- CloudTrail log-file validation enabled
- Multi-Region management-event collection
- GuardDuty enabled
- Security Hub enabled with AWS Foundational Security Best Practices
- EventBridge filtering for actionable findings
- SNS encryption using the AWS managed SNS KMS key
- No hardcoded credentials or account IDs

## Validate

```bash
cd terraform
terraform fmt -check -recursive
terraform init -backend=false
terraform validate
```

## Deploy

```bash
cd terraform
terraform init
terraform plan
terraform apply
```

A unique suffix is automatically added to the CloudTrail log bucket name.

> Deploying this reference implementation creates billable AWS resources. Review the Terraform plan and test in a non-production account first.

## Portfolio note

This repository is designed to demonstrate AWS cloud-security architecture, detection engineering, infrastructure as code, threat hunting, and incident-response patterns. It does not claim that the resources are currently deployed in a production AWS environment.