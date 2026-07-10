# Software Documentation Agent

## Purpose

This Claude Code project helps an agent read a software codebase and produce accurate, maintainable technical documentation. It is a **documentation toolkit**, not the software being documented. Point Claude at a target repository, then run `/document-project` (and optionally `/document-modules`) or delegate to the specialized subagents.

## Repo map

| Path | Role |
|------|------|
| `CLAUDE.md` | Project instructions loaded every session |
| `.claude/settings.json` | Shared permissions and defaults |
| `.claude/skills/document-project/SKILL.md` | Primary documentation prompt (`/document-project`) |
| `.claude/skills/document-project/templates/` | Output templates for generated docs |
| `.claude/skills/document-project/checklist.md` | Quality checklist before finishing |
| `.claude/skills/document-modules/SKILL.md` | Module decomposition prompt (`/document-modules`) |
| `.claude/skills/document-modules/templates/` | Template for `doc/modules.md` |
| `.claude/agents/codebase-doc-writer.md` | Subagent persona/tools/model for `/document-project` |
| `.claude/agents/module-doc-writer.md` | Subagent persona/tools/model for `/document-modules` |
| `.claude/rules/documentation-standards.md` | Writing standards and output conventions |

## How to use

### Option A — Document a project from this repo

1. Start Claude Code in this repository.
2. **Add the target codebase** (required when documenting outside this repo). Prefer a **relative path** on Windows (quoted absolute paths can break `/add-dir`):
   `/add-dir ..\..\Civia\rag-civia-mandate-service`
3. Run `/document-project` with the target path:
   `/document-project ..\..\Civia\rag-civia-mandate-service`
4. Confirm the agent reports **Files written** with absolute paths under `doc/`.
5. Review generated files under `doc/` in the target project.
6. (Optional) Decompose into modules after base docs exist:
   `/document-modules ..\..\Civia\rag-civia-mandate-service`
7. Confirm `doc/modules.md` was written.

**Important:** Skills write files to disk under `<target-project>/doc/` only. Never to the project root. If you only see an architecture summary in chat and no files on disk, the run did not complete — re-run after `/add-dir` and ensure you are not in plan-only mode.

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
6. **Output location.** Generated documentation **must** go in `doc/` at the target project root. Never write documentation to the project root or any other folder.
7. **Write files, not chat.** `/document-project` must use the Write tool to create `doc/README.md`, `doc/architecture.md`, `doc/development.md`, and `doc/api-reference.md`. `/document-modules` must write `doc/modules.md`. A chat-only summary is not a completed run.

## Default documentation set

When the user does not request a subset, `/document-project` generates under `doc/`:

- `doc/README.md` — Documentation index and quick start
- `doc/architecture.md` — System overview, components, data flow
- `doc/development.md` — Setup, build, test, and contribution workflow
- `doc/api-reference.md` — Public APIs, endpoints, or module surface (if applicable)

After base docs exist, `/document-modules` adds:

- `doc/modules.md` — Self-contained modules (functional + technical) with minimal inter-module dependencies

Use templates under `.claude/skills/document-project/templates/` and `.claude/skills/document-modules/templates/` as structure guides, not as filler text.

## Commands

| Command / agent | When to use |
|-----------------|-------------|
| `/document-project [path]` | Full documentation workflow (primary entry point) |
| `/document-modules [path]` | Module decomposition into `doc/modules.md` (requires prior `/document-project`) |
| `codebase-doc-writer` agent | Forked worker for `/document-project` (persona/tools; workflow is in the skill) |
| `module-doc-writer` agent | Forked worker for `/document-modules` (persona/tools; workflow is in the skill) |
