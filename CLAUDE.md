# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Claude Agents Forge is a collection of production-hardened agent specs (`.md` files with YAML frontmatter) for Claude Code. It is NOT a code project — there is no build system, no tests, no dependencies. The deliverables are agent specification files.

## Repository Structure

```
agents/
  orchestration/conductor.md    # Central orchestrator (opus, 200 turns)
  quality/                      # Read-only quality gate agents (sonnet)
  development/                  # Dev agents that write code (sonnet)
docs/
  agent-spec.md                 # Full YAML frontmatter reference
  comparison.md                 # Comparison with other agent repos
  getting-started.md            # Installation & usage guide
```

## Agent Spec Format

Every agent is a Markdown file with YAML frontmatter. All these fields are **required**:

```yaml
---
name: agent-name                    # lowercase, hyphens
description: >                      # role + capabilities + limitations
tools: [Read, Glob, Grep, Bash]     # explicit allowlist
disallowedTools: [Write, Edit, ...]  # hard denylist
model: sonnet                       # sonnet | opus | haiku | inherit
permissionMode: bypassPermissions   # default | bypassPermissions | plan
maxTurns: 60                        # hard turn limit
memory: local                       # user | project | local
mcpServers: [eslint, semgrep]       # optional MCP integrations
---
```

## Architecture Rules

- **Conductor** uses `opus`, never writes code, orchestrates via `Task()` references
- **Quality agents** (`agents/quality/`) are read-only — `Write`/`Edit`/`NotebookEdit` must be in `disallowedTools`
- **Dev agents** (`agents/development/`) must declare a "File Scope" section
- Dual-mode agents (test-engineer, doc-writer, monitoring-engineer) have explicit review/write modes
- Every agent must have: turn budget table, bash timeout table, memory strategy section, 75% turn limit safety trigger

## Verdict System

Quality agents return one of: `PASS`, `PASS WITH NOTES`, `NEEDS CHANGES`, `CRITICAL ISSUES`. The conductor reads the verdict line and routes accordingly. Max 3 correction round-trips per pipeline.

## When Creating a New Agent

1. Use an existing agent as a template (match the closest role)
2. Include all required frontmatter fields
3. Add turn budget table (phases summing to `maxTurns`, with ~10-15% reserve)
4. Add bash timeout table (5s file checks, 30s git, 60s static analysis, 120s tests)
5. Add memory strategy section (what to store in MEMORY.md vs Memory Service)
6. Quality agents: add `Write`/`Edit`/`NotebookEdit`/`Agent`/`WebSearch`/`WebFetch` to `disallowedTools`
7. Dev agents: add "File Scope" section listing allowed/forbidden directories
8. Add the agent to the conductor's `Task()` list if it should be orchestrated
9. Use "MCP first, Bash fallback" pattern for tool integrations

## Memory Scopes

| Scope | Who uses it | Shared |
|-------|-------------|--------|
| `user` | Conductor | All projects |
| `project` | Dev agents | Via git |
| `local` | Quality agents | Not shared |

## Stack-Adaptive Architecture (v1.1+)

Dev agents are **universal** — one `backend-dev` works with any backend framework. Stack-specific knowledge lives in `## Stack: <name>` sections inside the agent prompt. Agent detects the stack in 1-2 turns and follows the relevant profile.

**Why not separate agents per stack:** Duplicates ~70% content, splits memory across agents, bloats conductor's `Task()` list.

**Why not `skills:` field:** Skills have platform bugs (#29441, #17283) and don't work in Agent Teams (#30703). Embedded profiles work everywhere.

## Known Platform Bugs (affect architecture)

Only depend on features that reliably work. See `ROADMAP.md > Platform Bugs & Blockers` for full list.

- **Memory in subagents doesn't work** (#25318) — MEMORY.md is never saved. Prompts should handle missing memory gracefully.
- **`disallowedTools` doesn't block MCP tools** (#12863) — don't give write-capable MCP servers to read-only agents.
- **`disallowedTools` bypassed via Bash** — always add explicit Bash prohibitions in prompts ("NO sed -i, NO echo >").
- **MCP unavailable from `--agent` main thread** (#13898) — delegate MCP operations to subagents.
- **Use `PreToolUse` hooks** for reliable tool restriction enforcement (not just `disallowedTools`).

## Key Conventions

- Model choice: `opus` for orchestration, `sonnet` for all workers
- `permissionMode: bypassPermissions` for autonomous agents
- Subagents are one level deep only — conductor must run as main thread (`claude --agent conductor`)
- Agent names are kebab-case
- The project language is English (agent specs, docs, comments)
- Dev agents use stack profiles inside the prompt, not separate files per framework
