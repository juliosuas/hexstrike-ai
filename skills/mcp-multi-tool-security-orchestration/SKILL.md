---
name: mcp-multi-tool-security-orchestration
description: Orchestrate HexStrike's 150+ security tools via MCP protocol with intelligent dependency resolution, parallel execution, and cross-tool context sharing
domain: cybersecurity
subdomain: automation
tags:
  - mcp
  - orchestration
  - multi-tool
  - automation
  - penetration-testing
  - pipeline
version: "1.0"
author: juliosuas
license: Apache-2.0
---

## Overview

MCP Multi-Tool Security Orchestration enables AI agents to coordinate HexStrike's full arsenal of 150+ security tools through the Model Context Protocol. Instead of running tools sequentially with manual context transfer, this skill teaches agents to build dynamic execution graphs — resolving dependencies between tools, sharing discovered context (ports, services, credentials, vulnerabilities) across the pipeline, and making autonomous decisions about which tools to invoke next based on intermediate results.

The orchestration layer handles parallel execution where tools are independent, serializes where outputs feed inputs, and maintains a shared knowledge graph that every tool can read from and write to.

## When to Use

- Full-scope penetration tests requiring coordinated multi-tool execution
- Bug bounty workflows where recon → scanning → exploitation must chain seamlessly
- Red team engagements needing simultaneous network, web, cloud, and binary analysis
- Security assessments where tool selection should adapt based on discovered attack surface
- Any scenario requiring more than 3 tools to run with shared context

## Prerequisites

- HexStrike AI MCP Server running (`python3 hexstrike_server.py`)
- MCP-compatible AI client connected (Claude Desktop, Cursor, VS Code Copilot, Roo Code)
- Target systems with explicit written authorization for testing
- Core security tools installed (nmap, nuclei, gobuster minimum; full arsenal recommended)
- Python 3.8+ with HexStrike dependencies (`pip install -r requirements.txt`)

## Workflow

1. **Define Engagement Scope** — Provide the AI agent with target domains/IPs, authorized scope boundaries, and engagement rules. The orchestrator validates scope before any tool execution.

2. **Initialize Orchestration Context** — The agent creates a shared knowledge graph and registers all available tools with their input/output schemas. Tool health checks run in parallel to identify available capabilities.

3. **Generate Execution Plan** — Based on the target type (web app, network, cloud, API), the AI builds a directed acyclic graph (DAG) of tool invocations. Dependencies are resolved automatically — e.g., subfinder feeds httpx, which feeds nuclei.

4. **Execute Parallel Phases** — Independent tools run simultaneously. The orchestrator monitors all active processes, captures structured output, and updates the knowledge graph in real-time. Phase examples:
   - Phase 1 (Recon): subfinder + amass + fierce + dnsenum running in parallel
   - Phase 2 (Probing): httpx + nmap on discovered assets
   - Phase 3 (Scanning): nuclei + nikto + wpscan based on detected technologies
   - Phase 4 (Exploitation): sqlmap + dalfox on identified injection points

5. **Cross-Tool Context Enrichment** — As each tool completes, its findings enrich the shared context. Nuclei results annotate nmap-discovered services. Subdomain findings feed directory brute-forcing. The AI agent decides when new discoveries warrant additional tool invocations.

6. **Adaptive Re-planning** — If Phase 2 discovers unexpected services (e.g., a Redis instance, an exposed API gateway), the orchestrator dynamically adds relevant tools to subsequent phases without restarting the entire pipeline.

7. **Aggregate and Report** — All findings are correlated, deduplicated, severity-scored, and compiled into a unified assessment report with evidence chains linking discovery → validation → exploitation.

## Verification

| Check | How to Verify | Expected Result |
|-------|---------------|-----------------|
| MCP Connection | Agent lists available tools | 150+ tools visible in tool inventory |
| Parallel Execution | Monitor process dashboard during scan | Multiple tools running simultaneously |
| Context Sharing | Review knowledge graph entries | Findings from tool A appear in tool B's parameters |
| Dependency Resolution | Check execution order in logs | Dependent tools wait for prerequisite completion |
| Adaptive Re-planning | Introduce unexpected service in target | New tools added to pipeline dynamically |
| Scope Compliance | Review all executed commands | No out-of-scope targets contacted |
| Report Completeness | Open generated report | Findings include cross-tool evidence chains |
