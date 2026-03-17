---
name: mcp-social-engineering-analyzer
description: Map an organization's social engineering attack surface by correlating OSINT findings across email harvesting, social media exposure, and public data leaks to assess phishing and impersonation risk.
domain: cybersecurity
subdomain: social-engineering
tags: [mcp, osint, phishing, social-engineering, email-harvesting, reconnaissance, human-factor]
version: "1.0"
author: hexstrike-ai
license: MIT
tools: [theharvester, sherlock, amass, subfinder]
mcp_required: true
---
# MCP Social Engineering Analyzer

## Overview

The human layer is often the weakest link. This skill uses MCP-orchestrated OSINT tools to map an organization's social engineering attack surface: discoverable employee emails, social media exposure, leaked credentials, and organizational structure — then uses AI reasoning to assess phishing campaign viability and impersonation risk.

This goes beyond simple email harvesting. The AI agent correlates findings to build **target profiles** that identify high-value individuals and their likely susceptibility vectors.

## Prerequisites

- HexStrike AI MCP Server running
- OSINT tools: `theharvester`, `sherlock`, `amass`
- Authorization for social engineering assessment
- Target organization name and primary domain

## Key Concepts

### Attack Surface Dimensions

Social engineering exposure is measured across:
- **Email exposure**: How many employee emails are publicly discoverable?
- **Organizational structure**: Can reporting chains be mapped from public data?
- **Social media presence**: Employee profiles revealing roles, technologies, travel
- **Credential leaks**: Employee emails found in breach databases
- **Technical clues**: Error pages, job postings, GitHub commits revealing internal tech

### Phishing Viability Score

The AI agent generates a composite score (0-100) based on:
- Number of valid email addresses discovered
- Email format predictability (first.last vs. flast)
- SPF/DKIM/DMARC configuration strength
- Employee social media security hygiene
- Historical breach exposure

### Impersonation Risk Matrix

For each discovered employee, the agent assesses:
- Role visibility (C-suite, IT admin, finance = high risk)
- Social media exposure (photos, writing style samples available)
- Email domain spoofability (based on DMARC policy)

## Practical Steps

### Step 1: Email Enumeration

```
"Map the social engineering attack surface for example.com. Enumerate 
employee email addresses, identify the email naming convention, check 
email security controls, and assess phishing campaign viability."
```

### Step 2: OSINT Correlation

The agent orchestrates multiple tools:
- TheHarvester: Email and hostname harvesting from search engines
- Subfinder + Amass: Domain intelligence for mail server discovery
- DNS analysis: SPF, DKIM, DMARC record evaluation

### Step 3: Exposure Analysis

Correlate discovered information:
- Map email naming conventions to predict additional addresses
- Identify roles from public sources (LinkedIn titles in search results, about pages)
- Check discovered emails against known breach databases (with authorization)
- Evaluate mail security controls for spoofing resistance

### Step 4: Risk Report

Generate a social engineering risk assessment:
- Total email exposure count and sources
- Email security control grades (SPF: Pass/Fail, DMARC: Enforce/Monitor/None)
- High-risk individuals (C-suite with breached credentials, IT admins with social media exposure)
- Phishing viability score with contributing factors
- Recommended security awareness training focus areas

## Verification

- [ ] Email enumeration covers multiple OSINT sources
- [ ] Email naming convention is correctly identified
- [ ] SPF, DKIM, and DMARC records are evaluated and graded
- [ ] High-value targets are identified with role context
- [ ] Phishing viability score reflects actual exposure level
- [ ] Recommendations are specific to discovered weaknesses
- [ ] No unauthorized contact with discovered individuals

## Limitations

- OSINT quality depends on public data availability
- Breach database checking must be authorized and legal
- Social media analysis is limited to publicly visible information
- Results are point-in-time and may not reflect current employee roster
