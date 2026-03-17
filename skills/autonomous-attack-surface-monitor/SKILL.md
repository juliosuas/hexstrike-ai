---
name: autonomous-attack-surface-monitor
description: Continuously monitor an organization's external attack surface for changes — new subdomains, exposed services, certificate changes, and emerging vulnerabilities — with AI-driven alerting.
domain: cybersecurity
subdomain: attack-surface-management
tags: [mcp, monitoring, attack-surface, continuous, change-detection, alerting, autonomous]
version: "1.0"
author: hexstrike-ai
license: MIT
tools: [subfinder, httpx, nmap, nuclei, testssl]
mcp_required: true
---
# Autonomous Attack Surface Monitor

## Overview

Attack surfaces are not static. New subdomains appear, services get exposed, SSL certificates expire, and new CVEs affect running software. This skill configures HexStrike's MCP agent to perform **continuous, autonomous monitoring** of an organization's external perimeter, alerting on meaningful changes and automatically assessing new exposures.

Unlike one-shot scanning, this skill establishes a **baseline** and then detects **drift** — the delta between what was known and what exists now.

## Prerequisites

- HexStrike AI MCP Server running
- MCP-compatible AI agent with persistent session capability
- Tools: `subfinder`, `httpx`, `nmap`, `nuclei`, `testssl`
- Write access to output directory for baseline storage
- Target authorization for continuous monitoring

## Key Concepts

### Baseline-Drift Detection

The monitor works in two phases:
1. **Baseline scan**: Full reconnaissance to establish the known attack surface
2. **Delta scans**: Periodic re-scans that compare against the baseline

Changes are categorized by risk:
- 🔴 **Critical**: New high-severity vulnerability, exposed database port, expired SSL cert
- 🟡 **Warning**: New subdomain with web service, new open port, technology version change
- 🟢 **Info**: DNS record change, HTTP header modification, new redirect

### Autonomous Decision Loop

The AI agent decides what to investigate further based on change type:
- New subdomain → Full port scan + vulnerability check
- New open port → Service fingerprint + known exploit check
- Certificate change → Validity check + chain verification
- Technology change → CVE matching against new version

## Practical Steps

### Step 1: Establish Baseline

```
"Establish a security baseline for example.com. Enumerate all subdomains, 
scan live hosts for open ports and services, check SSL configurations, and 
save this as the reference state for future comparison."
```

### Step 2: Configure Monitoring Interval

The agent schedules periodic delta scans:
- **Subdomains**: Check every 6 hours (passive sources only)
- **Port scans**: Check every 24 hours (light scan, top 100 ports)
- **SSL certificates**: Check every 12 hours
- **Vulnerability templates**: Update and re-scan every 48 hours

### Step 3: Delta Analysis

On each cycle, the agent compares results against baseline:
- New assets are flagged and deep-scanned immediately
- Removed assets are logged (potential decommission or misconfiguration)
- Changed configurations are risk-assessed

### Step 4: Alert and Report

Changes above the configured threshold trigger alerts:
- Structured change report with before/after comparison
- Risk assessment for each change
- Recommended actions for critical changes

## Verification

- [ ] Baseline scan completed and stored as reference
- [ ] Delta scan correctly identifies intentionally added test subdomain
- [ ] New open ports are detected within one scan cycle
- [ ] SSL certificate expiry warnings fire at configured threshold
- [ ] Alert contains actionable change details (not just "something changed")
- [ ] Historical drift data is preserved for trend analysis

## Limitations

- Passive subdomain enumeration may miss internal-only or non-indexed subdomains
- Monitoring frequency is limited by tool execution time and rate limits
- Requires persistent storage for baseline data between sessions
