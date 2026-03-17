---
name: mcp-credential-audit-pipeline
description: Orchestrate a comprehensive credential security audit across network services, web applications, and cloud endpoints using coordinated MCP tool execution.
domain: cybersecurity
subdomain: authentication-security
tags: [mcp, credentials, password-audit, brute-force, default-credentials, authentication, multi-protocol]
version: "1.0"
author: hexstrike-ai
license: MIT
tools: [hydra, netexec, nmap, nuclei, evil-winrm]
mcp_required: true
---
# MCP Credential Audit Pipeline

## Overview

Weak, default, and reused credentials remain the #1 initial access vector in real-world breaches. This skill coordinates a multi-protocol credential audit through MCP, testing authentication across discovered services in a methodical, account-lockout-aware pipeline.

The key differentiator is **MCP-coordinated intelligence sharing**: when a credential is found on one service, it's immediately tested against all other discovered services — mimicking real attacker behavior of credential stuffing across an organization's entire exposed surface.

## Prerequisites

- HexStrike AI MCP Server running
- Completed reconnaissance with service enumeration
- Credential testing authorization (explicit written permission)
- Tools: `hydra`, `netexec`, `nmap`, `nuclei`
- Understanding of target's account lockout policies

## Key Concepts

### Lockout-Aware Testing

Before brute-forcing, the pipeline:
1. Identifies lockout policies through banner analysis and protocol probing
2. Sets attempt limits well below lockout thresholds
3. Spaces attempts across time to avoid triggering alerts
4. Tests common defaults and known leaked credentials before dictionary attacks

### Cross-Service Credential Propagation

When credentials are discovered on any service:
- Immediately queued for testing on ALL other discovered services
- Priority given to high-value targets (admin panels, databases, SSH)
- Password pattern analysis generates mutations for related systems

### Protocol-Specific Testing

Each protocol gets optimized testing strategies:
- **SSH/RDP**: Default credentials, then targeted wordlists
- **HTTP(S)**: Form-based auth, Basic auth, API key endpoints
- **SMB**: Null sessions, guest access, domain credential testing
- **Databases**: Default credentials per engine (postgres/postgres, sa/empty, root/empty)
- **SNMP**: Community string enumeration

## Practical Steps

### Step 1: Service-Credential Mapping

From recon results, build a map of authentication endpoints:

```
"From the reconnaissance results, identify all services requiring 
authentication. Map each to its protocol type and build a prioritized 
credential testing plan that respects lockout policies."
```

### Step 2: Default Credential Sweep

Test vendor defaults across all services first (highest ROI, lowest risk):
- Database defaults (postgres, mysql, mssql, mongodb, redis)
- Network device defaults (admin/admin, cisco/cisco)
- Application defaults (admin/password, tomcat/tomcat)
- SNMP community strings (public, private)

### Step 3: Targeted Testing

For remaining services, coordinate targeted credential testing:
- Leaked credential databases (with authorization)
- Organization-specific patterns (company name + year, seasonal patterns)
- Previously discovered credentials from other engagement phases

### Step 4: Credential Reuse Analysis

Cross-reference all discovered credentials:
- Test found credentials against every authenticated service
- Report on credential reuse patterns
- Identify shared service accounts

## Verification

- [ ] All authentication endpoints from recon are covered
- [ ] Account lockout policies identified and respected
- [ ] Default credential testing completed across all services
- [ ] Discovered credentials tested cross-service
- [ ] No accounts locked out during testing
- [ ] Credential reuse matrix generated
- [ ] Findings include protocol, host, username, and access level achieved

## Limitations

- Account lockout policies may severely limit testing speed
- Some services may not have detectable lockout policies
- Legal scope must explicitly authorize credential testing
- Cloud services may have additional monitoring that flags testing
