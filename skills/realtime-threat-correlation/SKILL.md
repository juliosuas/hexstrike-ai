---
name: realtime-threat-correlation
description: Correlate findings from active penetration testing with real-time threat intelligence feeds to prioritize vulnerabilities that are actively being exploited in the wild.
domain: cybersecurity
subdomain: threat-intelligence
tags: [mcp, threat-intel, correlation, cve, exploit-prediction, risk-prioritization, real-time]
version: "1.0"
author: hexstrike-ai
license: MIT
tools: [nuclei, nmap, searchsploit]
mcp_required: true
---
# Real-Time Threat Correlation

## Overview

Not all vulnerabilities are equal. A critical CVE with no public exploit is less urgent than a medium-severity bug with active exploitation campaigns. This skill correlates HexStrike scan results with live threat intelligence to produce **risk-prioritized findings** based on real-world exploitation activity.

The MCP agent enriches each finding with threat context: Is there a public exploit? Is it in CISA's KEV catalog? Are threat actors actively targeting this CVE? This transforms raw scan output into actionable intelligence.

## Prerequisites

- HexStrike AI MCP Server running
- Vulnerability scan results (from nuclei, nmap, or manual findings)
- Internet access for threat intelligence lookups
- Tools: `nuclei`, `nmap`, `searchsploit`

## Key Concepts

### Threat-Weighted Scoring

Each finding receives a composite score based on:
- **Base CVSS score** — Technical severity
- **Exploit availability** — Public PoC exists (Exploit-DB, GitHub)
- **Active exploitation** — CISA KEV listing, threat intel reports
- **Asset criticality** — Business importance of the affected system
- **Exposure level** — Internet-facing vs. internal-only

### Intelligence Sources

The agent queries multiple sources for each CVE:
- NIST NVD for CVSS and description
- CISA Known Exploited Vulnerabilities catalog
- Exploit-DB via searchsploit for public exploits
- GitHub advisory database
- Vendor security advisories

### Priority Matrix

Findings are bucketed into action categories:
- 🔴 **Patch Now**: Active exploitation + your system is vulnerable + internet-facing
- 🟠 **Patch Soon**: Public exploit exists + vulnerable service detected
- 🟡 **Schedule**: Known CVE, no public exploit yet
- 🟢 **Monitor**: Informational finding, low real-world risk

## Practical Steps

### Step 1: Extract CVE Identifiers

From scan results, extract all CVE references and affected software versions:

```
"Analyze the scan results for example.com. Extract all CVE identifiers 
and software versions. Cross-reference each with threat intelligence 
to determine real-world exploitation risk."
```

### Step 2: Threat Intelligence Enrichment

For each CVE, the agent gathers:
- CVSS v3.1 base score and vector
- Exploit availability (searchsploit, GitHub)
- CISA KEV listing status
- Days since public disclosure

### Step 3: Contextual Risk Scoring

Combine threat intelligence with asset context:
- Internet-facing services get a 2x risk multiplier
- Services handling sensitive data get additional weight
- Findings with active exploitation campaigns are auto-elevated to critical

### Step 4: Prioritized Report

Generate a risk-prioritized remediation plan:
- Ordered by composite risk score (not just CVSS)
- Each finding includes threat context and exploitation evidence
- Remediation timeline recommendations based on risk level

## Verification

- [ ] All CVEs from scan results are identified and enriched
- [ ] CISA KEV status is checked for each CVE
- [ ] Exploit availability is confirmed via searchsploit or equivalent
- [ ] Risk scores differ from raw CVSS (threat context changes priority)
- [ ] At least one finding's priority changed due to threat intelligence
- [ ] Remediation timeline aligns with threat-weighted priority
- [ ] Report distinguishes between theoretical and actively-exploited risks

## Limitations

- Threat intelligence is point-in-time; new exploits may emerge after analysis
- Some CVEs may not have NVD entries yet (zero-days, recent disclosures)
- Exploit availability doesn't guarantee exploitability in the target environment
