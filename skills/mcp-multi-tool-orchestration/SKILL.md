---
name: mcp-multi-tool-orchestration
description: Orchestrate 150+ security tools simultaneously through MCP protocol with intelligent dependency resolution, parallel execution, and cross-tool context sharing
domain: cybersecurity
subdomain: automation
tags:
  - mcp
  - orchestration
  - multi-tool
  - automation
  - penetration-testing
  - pipeline
version: 1.0.0
author: HexStrike AI
---

# MCP Multi-Tool Orchestration

Coordinate 150+ offensive and defensive security tools through a single MCP interface. The AI agent acts as an intelligent orchestrator — resolving tool dependencies, parallelizing independent tasks, sharing context between tools, and adapting execution plans in real time based on intermediate results.

## Why This Exists

Traditional pentesting requires manually chaining tools: run Nmap, parse output, feed ports to Nikto, take results to SQLMap, etc. This skill eliminates that friction entirely. The AI agent understands tool capabilities, data formats, and dependency graphs — orchestrating complex multi-tool workflows through natural language.

## Capabilities

- **Dependency-Aware Scheduling** — Automatically determines which tools can run in parallel vs. which need prior results
- **Cross-Tool Context Sharing** — Output from one tool enriches input for the next (e.g., Nmap service versions → targeted Nuclei templates)
- **Adaptive Re-Planning** — If a tool fails or returns unexpected results, the orchestrator adjusts the execution plan
- **Resource Management** — Prevents tool conflicts (port collisions, rate limiting, target overload)
- **Execution Telemetry** — Real-time progress tracking across all running tools with estimated completion

## Example Workflow

```
User: "Run a comprehensive assessment of target.example.com"

Orchestrator Plan:
  Phase 1 (parallel):
    ├── nmap_tcp_full → port/service discovery
    ├── subfinder + httpx → subdomain enumeration
    ├── whatweb → technology fingerprinting
    └── wafw00f → WAF detection
  
  Phase 2 (depends on Phase 1):
    ├── nuclei (templates selected from Phase 1 tech stack)
    ├── gobuster (paths, filtered by WAF rules)
    ├── nikto (per discovered web service)
    └── sslscan (per HTTPS endpoint)
  
  Phase 3 (depends on Phase 2):
    ├── sqlmap (on discovered injection points)
    ├── xsstrike (on reflected parameters)
    └── credential_audit (on discovered login forms)
  
  Final: Correlated report with cross-tool evidence chains
```

## Key Differentiator

Unlike static pipeline tools (e.g., bash scripts chaining commands), MCP Multi-Tool Orchestration uses **AI reasoning** at every decision point. The agent doesn't just run tools — it interprets results, identifies false positives, discovers attack paths humans might miss, and dynamically adjusts scope based on risk signals.

## Verification

| Check | Method | Expected |
|-------|--------|----------|
| Tool inventory | List available MCP tools | 150+ tools registered |
| Parallel execution | Run multi-phase scan | Tools execute concurrently where safe |
| Dependency resolution | Check execution order | Dependent tools wait for prerequisites |
| Context propagation | Verify tool inputs | Later tools reference earlier findings |
| Adaptive replanning | Simulate tool failure | Plan adjusts without user intervention |
