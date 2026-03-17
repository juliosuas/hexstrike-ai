---
name: mcp-security-pipeline-builder
description: Build, customize, and deploy reusable security testing pipelines via MCP using natural language — from ad-hoc scans to CI/CD-integrated security gates
domain: cybersecurity
subdomain: automation
tags:
  - mcp
  - pipeline
  - ci-cd
  - automation
  - security-testing
  - devsecops
version: 1.0.0
author: HexStrike AI
---

# MCP Security Pipeline Builder

Design and deploy custom security testing pipelines through natural language. Describe what you want to test and how — the AI translates your intent into executable MCP tool chains with proper sequencing, error handling, output formatting, and optional CI/CD integration. Pipelines are reusable, versionable, and shareable.

## Why This Exists

Every organization has unique security testing needs. Building automated pipelines traditionally requires scripting expertise, tool-specific knowledge, and significant maintenance overhead. This skill lets security teams describe pipelines in plain English and get production-ready automation — from quick ad-hoc checks to full CI/CD security gates.

## Capabilities

- **Natural Language → Pipeline** — Describe your testing workflow; AI generates the executable pipeline
- **Template Library** — Pre-built pipelines for common scenarios (OWASP Top 10, API security, cloud audit, compliance)
- **Conditional Logic** — Pipelines branch based on findings (e.g., "if SQLi found, escalate to full exploitation")
- **Output Normalization** — Unified finding format regardless of which tools produced the data
- **CI/CD Integration** — Export pipelines as GitHub Actions, GitLab CI, or Jenkins stages
- **Pipeline Versioning** — Track changes to pipelines over time, diff between versions
- **Dry Run Mode** — Preview what a pipeline will execute without actually running tools

## Pipeline Definition Example

```yaml
# Generated from: "Build me a web app security pipeline 
# that checks OWASP Top 10 and blocks deployment on critical findings"

name: owasp-top10-security-gate
trigger: pre-deployment
target: ${APP_URL}
fail_on: critical

stages:
  - name: discovery
    parallel: true
    tools:
      - httpx: { tech_detect: true, status_code: true }
      - whatweb: { aggression: 3 }
      - wafw00f: {}

  - name: vulnerability_scan
    depends_on: discovery
    parallel: true
    tools:
      - nuclei: { templates: "owasp-top-10", severity: "medium,high,critical" }
      - nikto: { tuning: "1,2,3,4,5,7,9" }
      - zap_scan: { policy: "owasp-top-10" }

  - name: injection_testing
    depends_on: vulnerability_scan
    condition: "vulnerability_scan.findings.injection_points > 0"
    tools:
      - sqlmap: { level: 3, risk: 2, batch: true }
      - xsstrike: { crawl: true }
      - commix: { level: 2 }

  - name: auth_testing
    depends_on: discovery
    tools:
      - credential_audit: { wordlist: "top-1000" }
      - jwt_tool: { scan_mode: true }

  - name: report
    depends_on: all
    format: [json, html, sarif]
    gate_check: true  # Fail pipeline if critical findings exist
```

## Pre-Built Pipeline Templates

| Template | Use Case | Tools Involved |
|----------|----------|---------------|
| `web-app-full` | Comprehensive web application test | 15+ tools, 4 phases |
| `api-security` | REST/GraphQL API assessment | nuclei, jwt_tool, paramspider |
| `cloud-audit` | AWS/GCP/Azure misconfiguration | prowler, scoutsuite, cloudsploit |
| `network-sweep` | Internal/external network assessment | nmap, masscan, enum4linux |
| `compliance-pci` | PCI-DSS compliance checks | Custom nuclei + config audits |
| `bug-bounty-quick` | Fast recon + low-hanging fruit | subfinder, httpx, nuclei |

## Key Differentiator

Existing pipeline tools (e.g., bash scripts, Ansible playbooks) are rigid and require maintenance. MCP Security Pipeline Builder uses AI reasoning to generate **intelligent** pipelines — the AI understands tool capabilities and limitations, avoids redundant checks, selects appropriate tool configurations based on target context, and produces pipelines that adapt to what they discover during execution.

## Verification

| Check | Method | Expected |
|-------|--------|----------|
| NL → Pipeline | Describe pipeline in English | Valid executable pipeline generated |
| Template execution | Run pre-built template | All stages complete successfully |
| Conditional branching | Trigger pipeline with injection points | Injection stage activates |
| CI/CD export | Export as GitHub Action | Valid workflow YAML produced |
| Dry run | Execute with dry_run flag | Execution plan shown, no tools run |
| Gate check | Run against vulnerable target | Pipeline fails on critical findings |
