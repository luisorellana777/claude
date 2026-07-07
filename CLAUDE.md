# Software Documentation Agent

## Purpose

This Claude Code project helps an agent read a software codebase and produce accurate, maintainable technical documentation. It is a **documentation toolkit**, not the software being documented. Point Claude at a target repository, then run `/document-project` or delegate to the `codebase-doc-writer` subagent.

## Repo map

| Path | Role |
|------|------|
| `CLAUDE.md` | Project instructions loaded every session |
| `.claude/settings.json` | Shared permissions and defaults |
| `.claude/skills/document-project/SKILL.md` | Primary documentation prompt (`/document-project`) |
| `.claude/skills/document-project/templates/` | Output templates for generated docs |
| `.claude/skills/document-project/checklist.md` | Quality checklist before finishing |
| `.claude/agents/codebase-doc-writer.md` | Specialized subagent for large codebases |
| `.claude/rules/documentation-standards.md` | Writing standards and output conventions |

## How to use

### Option A — Document a project from this repo

1. Start Claude Code in this repository.
2. Add the target codebase: `/add-dir /path/to/target-project`
3. Run `/document-project` with an optional path: `/document-project /path/to/target-project`
4. Review generated files under `docs/` in the target project (or a path you specify).

### Option B — Copy config into the target repo

1. Copy `.claude/` and `CLAUDE.md` into the root of the project you want to document.
2. Start Claude Code from that project root.
3. Run `/document-project`.

## Rules for every documentation session

1. **Read before writing.** Explore the codebase with Read, Grep, and Glob before producing any documentation. Do not invent APIs, modules, or behavior.
2. **Cite evidence.** Every architectural claim should trace to files you inspected (paths, symbols, or config).
3. **Match the stack.** Detect languages, frameworks, and build tools from the repo; do not assume a stack.
4. **Write for developers.** Prefer clear explanations, diagrams where helpful, and runnable commands verified against project config.
5. **Minimal scope.** Document what exists; mark unknowns as `TBD` or `Not found in codebase` instead of guessing.
6. **Output location.** Default output is `docs/` at the target project root unless the user specifies otherwise.

## Default documentation set

When the user does not request a subset, generate:

- `docs/README.md` — Documentation index and quick start
- `docs/architecture.md` — System overview, components, data flow
- `docs/development.md` — Setup, build, test, and contribution workflow
- `docs/api-reference.md` — Public APIs, endpoints, or module surface (if applicable)

Use templates in `.claude/skills/document-project/templates/` as structure guides, not as filler text.

## Commands

| Command / agent | When to use |
|-----------------|-------------|
| `/document-project [path]` | Full documentation workflow (primary entry point) |
| `codebase-doc-writer` agent | Large repos; keeps exploration out of the main thread |
