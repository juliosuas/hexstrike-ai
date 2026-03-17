---
name: mcp-multi-tool-security-orchestration
description: Orchestrate 150+ cybersecurity tools through a unified MCP interface, enabling AI agents to chain reconnaissance, scanning, exploitation, and reporting tools into automated security workflows without manual tool switching.
domain: cybersecurity
subdomain: penetration-testing
tags: [mcp, orchestration, automation, multi-tool, security-pipeline, ai-agent]
version: "1.0"
author: juliosuas
license: MIT
---

# MCP Multi-Tool Security Orchestration

## When to Use
- Running multi-phase security assessments requiring 5+ tools
- Building automated scan-to-report pipelines
- Coordinating reconnaissance → scanning → exploitation workflows
- When an AI agent needs to autonomously select and chain security tools

## Prerequisites
- HexStrike AI MCP Server running (`python hexstrike_server.py`)
- MCP-compatible AI client (Claude Code, Cursor, etc.)
- Network access to target systems (authorized testing only)

## Workflow
1. Initialize HexStrike MCP connection from AI client
2. Define target scope and engagement rules
3. AI agent queries available tools via `mcp.tools.list`
4. Agent selects reconnaissance tools based on target type (web, network, cloud)
5. Chain tool outputs: nmap results → vulnerability scanner → exploit framework
6. Aggregate findings into unified JSON report
7. Generate executive summary with risk scoring

## Verification
- Confirm MCP handshake completes successfully
- Verify tool chain produces correlated output (findings reference same targets)
- Validate report contains findings from all chained tools
- Check no tool executed outside authorized scope
