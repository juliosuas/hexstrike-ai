---
name: ai-cloud-posture-scanner
description: AI-driven multi-cloud security posture assessment that orchestrates cloud security tools via MCP and uses reasoning to identify misconfigurations that automated scanners miss.
domain: cybersecurity
subdomain: cloud-security
tags: [mcp, cloud, aws, azure, gcp, posture-management, misconfiguration, iam, compliance]
version: "1.0"
author: hexstrike-ai
license: MIT
tools: [prowler, scout-suite, trivy, kube-hunter, kube-bench]
mcp_required: true
---
# AI Cloud Posture Scanner

## Overview

Cloud misconfigurations are responsible for the majority of cloud breaches. Tools like Prowler and ScoutSuite generate hundreds of findings, but lack the reasoning to prioritize by actual exploitability. This skill combines MCP-orchestrated cloud scanning with AI analysis to identify the misconfigurations that actually matter — and explain why.

## Prerequisites

- HexStrike AI MCP Server running
- Cloud provider credentials configured (AWS CLI, Azure CLI, or gcloud)
- Tools: `prowler`, `scout-suite`, `trivy`
- For Kubernetes: `kube-hunter`, `kube-bench`
- Authorization for cloud security assessment

## Key Concepts

### Context-Aware Prioritization

The AI agent doesn't just list misconfigurations — it chains them:
- S3 bucket is public → contains CloudFormation templates → templates reference IAM roles with admin permissions
- Security group allows 0.0.0.0/0 on port 22 → EC2 instance runs outdated SSH → key-based auth disabled

### Multi-Cloud Normalization

Findings across AWS, Azure, and GCP are normalized into a common taxonomy:
- Storage exposure (S3 / Azure Blob / GCS)
- Identity over-privilege (IAM / Azure AD / GCP IAM)
- Network exposure (Security Groups / NSG / Firewall Rules)
- Encryption gaps (KMS / Key Vault / Cloud KMS)

## Practical Steps

### Step 1: Cloud Environment Discovery

```
"Assess the cloud security posture for our AWS account. Run Prowler 
for compliance checks and ScoutSuite for configuration review. 
Analyze results for exploitable misconfiguration chains."
```

### Step 2: Automated Scanning

The agent orchestrates cloud tools in parallel:
- Prowler: CIS benchmark compliance, PCI DSS, HIPAA checks
- ScoutSuite: IAM, storage, compute, network configuration review
- Trivy: Container image and IaC scanning

### Step 3: AI-Driven Analysis

The agent reasons about findings in context:
- Which public resources contain sensitive data?
- Which over-privileged roles are actually in use?
- What's the blast radius of each misconfiguration?
- Which findings combine into exploitation chains?

### Step 4: Remediation Plan

Prioritized fixes with infrastructure-as-code snippets:
- Terraform/CloudFormation templates for critical fixes
- IAM policy recommendations (least-privilege rewrites)
- Network segmentation suggestions

## Verification

- [ ] Cloud scanning tools completed without credential errors
- [ ] Findings cover IAM, storage, network, and compute categories
- [ ] AI analysis identifies at least one misconfiguration chain
- [ ] Prioritization differs from raw tool output (AI reasoning applied)
- [ ] Remediation includes actionable IaC snippets
- [ ] Compliance mapping shows relevant framework coverage (CIS, SOC 2, etc.)

## Limitations

- Requires appropriate cloud credentials with read-only access minimum
- Some checks require specific API permissions that may not be granted
- Multi-account environments need per-account credential configuration
- Real-time cloud state may change between scan and analysis
