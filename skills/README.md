# ⚔️ HexStrike AI Skills

> **agentskills.io compatible** — Each skill uses YAML frontmatter for progressive discovery.  
> AI agents read ~50 tokens of metadata to decide relevance, then load the full body on demand.

## Skill Categories

| Category | Skills | Description |
|----------|--------|-------------|
| 🔍 Reconnaissance | 2 | MCP-orchestrated multi-tool recon and attack surface monitoring |
| 🧠 AI Analysis | 2 | Autonomous vulnerability chaining and threat correlation |
| ☁️ Cloud Security | 1 | AI-driven cloud posture assessment |
| 🔐 Credential Security | 1 | Multi-protocol credential audit pipeline |
| 🎭 Social Engineering | 1 | AI-powered social engineering surface analysis |

## How Skills Work

HexStrike skills leverage the **Model Context Protocol (MCP)** to orchestrate multiple security tools through a single AI agent interface. Unlike traditional skill definitions, these skills:

1. **Orchestrate multiple tools** — A single skill may coordinate 5-10 underlying tools via MCP
2. **Adapt in real-time** — The AI agent adjusts parameters based on intermediate results
3. **Chain autonomously** — Skills can invoke other skills to build complete attack narratives

## Compatibility

These skills are designed for any MCP-compatible AI agent:

| Platform | Status | Notes |
|----------|--------|-------|
| Claude Desktop | ✅ Full | Native MCP support |
| VS Code Copilot | ✅ Full | Via MCP extension |
| Cursor | ✅ Full | Built-in MCP |
| Roo Code | ✅ Full | MCP compatible |
| Custom MCP Clients | ✅ Full | Any FastMCP consumer |
| Standalone CLI | ⚠️ Partial | Requires MCP bridge |

## Adding New Skills

Create a directory under `skills/` with:
- `SKILL.md` — YAML frontmatter + full skill documentation
- `references/` — Supporting materials (optional)
- `assets/` — Templates, scripts (optional)

See any existing skill for the format template.
