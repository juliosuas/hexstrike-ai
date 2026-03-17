---
name: real-time-attack-surface-monitoring
description: Continuous monitoring of external attack surfaces with baseline drift detection, change alerting, and AI-driven risk assessment of new exposures
domain: cybersecurity
subdomain: monitoring
tags:
  - attack-surface
  - monitoring
  - continuous
  - drift-detection
  - real-time
  - exposure-management
version: 1.0.0
author: HexStrike AI
---

# Real-Time Attack Surface Monitoring

Continuous, AI-powered monitoring of your external attack surface. Establishes a baseline of known assets, services, and configurations — then detects and risk-scores any drift in real time. New subdomain appeared? Port opened? TLS certificate changed? The AI evaluates the security implications and alerts with context, not just noise.

## Why This Exists

Attack surfaces change constantly — new deployments, cloud instances, forgotten subdomains, shadow IT. Point-in-time assessments miss changes that happen between scans. This skill provides persistent visibility with intelligent alerting that distinguishes routine changes from genuine security risks.

## Capabilities

- **Baseline Establishment** — Comprehensive initial scan creates a fingerprint of the entire external surface
- **Continuous Drift Detection** — Periodic re-scans compared against baseline to detect changes
- **AI Risk Scoring** — Every change is evaluated by the AI for security impact (not just "something changed")
- **Change Classification** — Categorizes changes: new asset, removed asset, service change, config drift, certificate rotation
- **Alert Prioritization** — High-risk changes (new admin panels, exposed databases, cert expirations) surface immediately; routine changes are logged silently
- **Historical Trend Analysis** — Track attack surface growth/shrinkage over time

## Monitoring Vectors

```
Continuous Checks:
  ├── Subdomain enumeration (new/removed domains)
  ├── Port/service monitoring (opened/closed/changed)
  ├── TLS certificate tracking (expiry, issuer changes, SANs)
  ├── HTTP response fingerprinting (tech stack changes)
  ├── DNS record monitoring (A, CNAME, MX, TXT changes)
  ├── Cloud asset discovery (new IPs, buckets, endpoints)
  └── Content change detection (login pages, error messages)

AI Risk Assessment per Change:
  ├── Is this an authorized change? (correlate with change windows)
  ├── Does this increase attack surface? (new exposure = higher risk)
  ├── Does this introduce known vulnerabilities? (version → CVE lookup)
  ├── Is this consistent with the organization's tech stack?
  └── Severity: Critical / High / Medium / Low / Informational
```

## Example Alert

```
🔴 CRITICAL CHANGE DETECTED
  Asset: staging-api.target.com
  Change: New port 27017 (MongoDB) exposed to internet
  Previous: Not in baseline
  Risk: Unauthenticated MongoDB access possible
  AI Assessment: "This MongoDB instance was not previously exposed. 
    Default MongoDB installations allow unauthenticated access. 
    Combined with the staging environment context, this likely 
    contains test data that may mirror production schemas."
  Recommended Action: Verify authentication, check for data exposure
```

## Key Differentiator

Traditional attack surface monitoring tools generate noise — "port 443 still open" is not actionable. This skill uses AI to evaluate **security significance** of every change. The agent understands that a new MongoDB port on a staging server is critical, while a routine certificate renewal is informational. Context-aware alerting, not checkbox scanning.

## Verification

| Check | Method | Expected |
|-------|--------|----------|
| Baseline creation | Run initial scan | Complete asset inventory stored |
| Drift detection | Modify target (add subdomain) | Change detected on next cycle |
| Risk scoring | Introduce high-risk change | Alert with severity and context |
| False positive rate | Monitor stable target | No spurious alerts over 24h |
| Historical tracking | Query change history | Timeline of all detected changes |
