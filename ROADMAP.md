# Roadmap

Future improvements for claude-agents-forge. Contributions welcome — pick any item and open a PR.

---

## v1.1 — More Agents

- [ ] **database-engineer** — schema design, migrations, query optimization, index analysis
- [ ] **api-designer** — OpenAPI spec review, REST/GraphQL best practices, versioning
- [ ] **refactoring-agent** — code smell detection, extract method/class, dead code removal
- [ ] **dependency-auditor** — outdated packages, license compliance, vulnerability scanning
- [ ] **i18n-reviewer** — missing translations, hardcoded strings, locale consistency
- [ ] **accessibility-auditor** — WCAG compliance, aria attributes, keyboard navigation

## v1.2 — Stack-Specific Dev Agents

- [ ] **rails-dev** — Ruby on Rails backend (models, migrations, jobs, admin)
- [ ] **django-dev** — Python/Django backend
- [ ] **nestjs-dev** — NestJS/TypeScript backend
- [ ] **go-dev** — Go backend (net/http, gin, fiber)
- [ ] **flutter-dev** — Dart/Flutter mobile development
- [ ] **unity-dev** — C# / Unity game development

## v2.0 — Agent Teams Integration

- [ ] Agent Teams support (experimental Claude Code feature)
- [ ] Team Lead agent that coordinates parallel dev agents
- [ ] Shared task list between teammates
- [ ] Direct mailbox communication between agents
- [ ] Conflict resolution protocol for overlapping file changes

## v2.1 — Advanced Orchestration

- [ ] **Multi-stage pipelines** — plan → implement → test → review → deploy
- [ ] **Conditional gates** — skip security audit if CSS-only change
- [ ] **Parallel dev agents** — frontend + backend simultaneously with file locking
- [ ] **Retry policies** — configurable retry count per agent, exponential backoff
- [ ] **Pipeline templates** — predefined pipelines for common workflows (bug fix, feature, refactor)
- [ ] **Cost tracking** — estimate token usage per pipeline run

## v2.2 — Memory & Learning

- [ ] **MCP Memory Service integration** — structured long-term memory via MCP (SQLite-vec, semantic search)
- [ ] **Memory-aware prompts** — agents consult memory before starting and save findings after
- [ ] **Project fingerprinting** — auto-detect stack, conventions, linting config and persist to memory
- [ ] **Cross-session learning** — agents remember project patterns, past review findings, recurring issues
- [ ] **Team knowledge base** — shared memory between quality agents (e.g., reviewer learns what auditor found)
- [ ] **Memory scoping strategy** — guidelines for what goes into user / project / local memory
- [ ] **Auto-calibration** — agents adjust review depth based on past false positive rate
- [ ] **Pattern library** — reusable review checklists per framework, stored in memory
- [ ] **Memory export/import** — share learned patterns between projects
- [ ] **Memory hygiene** — automatic cleanup of stale/conflicting memories, deduplication
- [ ] **Onboarding memory** — first-run agent populates memory with project structure, key files, dependencies

## v3.0 — Plugin System

- [ ] Package as Claude Code plugin (plugin.json manifest)
- [ ] One-command installation via plugin registry
- [ ] Plugin settings UI for selecting which agents to enable
- [ ] Agent marketplace — community-contributed agents with ratings
- [ ] Plugin hooks for custom pre/post agent actions

## v3.1 — CI/CD Integration

- [ ] **GitHub Actions workflow** — run quality gates on PR
- [ ] **GitLab CI template** — same for GitLab
- [ ] **Pre-commit hook** — local quality check before push
- [ ] **PR comment bot** — post agent verdicts as PR comments
- [ ] **Status checks** — block merge on CRITICAL verdicts
- [ ] **Diff-only mode** — review only changed lines, not full files

## v3.2 — Reporting & Analytics

- [ ] **HTML report** — visual quality report with charts
- [ ] **Trend tracking** — quality score over time per project
- [ ] **Verdict history** — track which issues keep recurring
- [ ] **Agent performance metrics** — turns used, time taken, verdict accuracy
- [ ] **Dashboard** — web UI for monitoring agent runs across projects

## Ongoing

- [ ] Better turn budget optimization (measure actual usage per phase)
- [ ] More MCP server integrations (Prettier, Biome, Ruff, mypy, Clippy)
- [ ] Windows-native improvements (PowerShell compatibility)
- [ ] Agent prompt compression (reduce token usage without losing quality)
- [ ] Benchmark suite — reproducible test cases to measure agent quality
- [ ] Multilingual documentation (Russian, Chinese, Japanese, Spanish)

---

## How to Contribute

Pick any unchecked item, open an issue to discuss the approach, then submit a PR. See [CONTRIBUTING.md](CONTRIBUTING.md) for agent quality standards.
