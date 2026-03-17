---
name: mcp-recon-orchestrator
description: Orchestrate comprehensive reconnaissance across multiple tools simultaneously via MCP, correlating results in real-time to build a unified attack surface map.
domain: cybersecurity
subdomain: reconnaissance
tags: [mcp, reconnaissance, orchestration, attack-surface, subdomain-enum, port-scan, osint, multi-tool]
version: "1.0"
author: hexstrike-ai
license: MIT
tools: [nmap, subfinder, amass, httpx, nuclei, theharvester, dnsenum]
mcp_required: true
---
# MCP Reconnaissance Orchestrator

## Overview

Traditional reconnaissance runs tools sequentially — subdomain enumeration, then port scanning, then service detection. The MCP Recon Orchestrator runs them **concurrently through the MCP protocol**, feeding results between tools in real-time. When subfinder discovers a subdomain, it's immediately queued for port scanning and HTTP probing without waiting for the full enumeration to complete.

This skill leverages HexStrike's unique MCP multi-tool orchestration to reduce recon time by 60-80% compared to sequential approaches.

## Prerequisites

- HexStrike AI MCP Server running (`python3 hexstrike_server.py`)
- MCP-compatible AI agent (Claude Desktop, Cursor, VS Code Copilot)
- Network tools installed: `nmap`, `subfinder`, `amass`, `httpx`
- Target authorization documented

## Key Concepts

### Parallel Tool Orchestration

The MCP protocol allows the AI agent to maintain multiple active tool sessions simultaneously. Rather than invoking tools one at a time, the orchestrator:

1. Launches subdomain enumeration (subfinder + amass) in parallel
2. Streams discovered subdomains to httpx for live host detection
3. Feeds live hosts to nmap for service fingerprinting
4. Routes interesting services to nuclei for vulnerability checks
5. Correlates all results into a unified target profile

### Adaptive Depth Control

The AI agent monitors result density and adjusts scanning depth dynamically:
- **High-density targets** (100+ subdomains): Broad, shallow scans first, then deep-dive on interesting hosts
- **Low-density targets** (<20 subdomains): Comprehensive deep scanning on all discovered assets
- **Rate-limited targets**: Automatic throttling and source rotation

## Practical Steps

### Step 1: Initialize the Orchestration Context

Prompt your MCP agent with target scope and authorization:

```
"I have written authorization to test example.com. Run a full MCP-orchestrated 
reconnaissance: enumerate subdomains, probe live hosts, scan ports, and identify 
services. Correlate everything into a unified attack surface map."
```

### Step 2: Parallel Enumeration Phase

The agent will invoke multiple HexStrike MCP tools concurrently:
- `subfinder_enum(target)` — Passive subdomain discovery
- `amass_enum(target)` — Active + passive enumeration
- `dnsenum_scan(target)` — DNS record enumeration
- `theharvester_scan(target)` — Email and hostname harvesting

### Step 3: Live Host Resolution

As subdomains stream in, the agent immediately probes them:
- `httpx_probe(subdomains)` — HTTP/HTTPS availability check
- Results feed directly into the next phase

### Step 4: Service Fingerprinting

Live hosts are port-scanned with intelligent defaults:
- `nmap_scan(hosts, "-sV -sC --top-ports 1000")` — Service version detection
- The AI agent adjusts scan intensity based on response patterns

### Step 5: Initial Vulnerability Check

High-value services trigger immediate vulnerability scanning:
- `nuclei_scan(hosts, "-severity critical,high")` — Known vulnerability detection
- Results are correlated with service versions for accuracy

## Expected Output

The orchestrator produces a structured attack surface map:

```
Target: example.com
├── Subdomains: 47 discovered, 31 live
├── Services: 89 open ports across 31 hosts
├── Technologies: Apache 2.4, nginx 1.22, Node.js 18, PostgreSQL 15
├── Vulnerabilities: 3 critical, 7 high, 12 medium
└── Attack Vectors: 4 prioritized chains identified
```

## Verification

To confirm successful execution:

- [ ] Multiple recon tools ran concurrently (check MCP process list)
- [ ] Subdomain count matches or exceeds single-tool baseline
- [ ] All live hosts have port scan results
- [ ] Service versions are identified for open ports
- [ ] Results are correlated (no orphaned findings)
- [ ] Attack surface map is generated with prioritized vectors
- [ ] Execution time is significantly less than sequential approach

## Limitations

- Requires all underlying tools to be installed on the host
- Network-intensive; ensure you have authorization for scan volumes
- Rate limiting on passive sources may reduce subdomain coverage
- Some tools have overlapping coverage — deduplication is handled by the orchestrator
