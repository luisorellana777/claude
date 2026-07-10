---
name: document-project
description: Analyze a software codebase and generate technical documentation (architecture, development guide, API reference). Use when the user asks to document a project, create docs for a repository, or run the documentation workflow.
when_to_use: User says "document this project", "create technical documentation", "write docs for this repo", or provides a path to a codebase to document.
argument-hint: [target-project-path]
arguments:
  - target-path
disable-model-invocation: true
context: fork
agent: codebase-doc-writer
allowed-tools: Read Grep Glob Write Edit Bash
---

# Document software project

You are documenting a software project. Follow this workflow exactly. Do not skip discovery; do not invent details.

**Division of responsibility:** This skill owns the workflow, paths, templates, checklist, and report. The forked agent `codebase-doc-writer` supplies persona, tools, and model — it must execute *this* prompt, not a parallel copy of these steps.

## Critical — depth and diagrams (mandatory)

Shallow summaries are a **failed** run. Documentation must be useful to an engineer onboarding to the codebase.

1. **Deep analysis:** Read controllers, services/facades, repositories, AI/clients, security config, application config, Docker/K8s when present — not only the README and one entry class.
2. **Architecture depth:** Every major component gets a full subsection (responsibility, collaborators, I/O, failure modes, evidence). Include **at least three** mermaid diagrams in `architecture.md`:
   - system context
   - component / layer diagram
   - at least one sequence diagram for a primary flow
   - plus ER or deployment diagram when data model or deploy manifests exist
3. **API depth:** Document **every** public HTTP endpoint (or CLI/library export) with request/response fields from DTOs/handlers — not a table-only index.
4. **Development depth:** Include profiles/env, local dependencies, project layout diagram, where-to-change map, Docker/Compose/K8s when evidenced, and concrete troubleshooting.
5. **No filler:** Prefer evidence-backed detail over generic Spring/RAG boilerplate. If something is unknown, mark `TBD` / `Not found in codebase`.
6. **Length heuristic:** `architecture.md` and `api-reference.md` should each be substantial (typically multiple screens of content for a non-trivial service). A one-page architecture for a multi-package service is insufficient.

## Critical — file output is mandatory

**This task is NOT complete until documentation files exist on disk.**

- You **MUST** use the **Write** tool to create every documentation file listed in Phase 3.
- Do **NOT** paste full documentation in chat instead of writing files.
- Do **NOT** stop after Phase 1 or Phase 2 with an architecture summary in chat.
- Phase 5 is a short report only — the real deliverable is the files on disk.
- If any required file is missing after Phase 3, go back and write it before finishing.

## Critical — output location is fixed: `doc/`

**All generated documentation MUST live under `<project-path>/doc/` — never anywhere else.**

- Output folder is **always** `doc` (not `docs`, not a custom name, not the project root).
- **Never** write documentation files to the project root (e.g. no `ARCHITECTURE.md`, `DEVELOPMENT.md`, or `API_DOCUMENTATION.md` at `<project-path>/`).
- **Never** create a folder named `civia-documentation`, `custom-docs`, or any other output directory.
- If the user passes a second argument, **ignore it** — output is always `doc/`.
- Use these exact paths only:
  - `<project-path>/doc/README.md`
  - `<project-path>/doc/architecture.md`
  - `<project-path>/doc/development.md`
  - `<project-path>/doc/api-reference.md`

## Target

Resolve paths before discovery:

- **Project path:** `$target-path` (if empty, use the repository root or the path from `/add-dir`)
- **Output folder:** `doc` (fixed — do not change)
- **Absolute output path:** `<project-path>/doc/`
- **Arguments (raw):** $ARGUMENTS

### Path rules

1. **Prefer relative paths on Windows** for `/add-dir` and `/document-project`. Quoted absolute paths can be concatenated incorrectly to the cwd (e.g. `...\claude"C:\Users\...`).
2. Example from this toolkit repo:
   `/add-dir ..\..\Civia\rag-civia-mandate-service`
3. When the target project is **outside** the current working directory, ensure it is accessible via `/add-dir`, `--add-dir` at startup, or `permissions.additionalDirectories` in settings.
4. Use **absolute paths** in Write tool calls. Every Write path must start with `<project-path>/doc/`.
5. For Bash on Windows paths with spaces, always `cd` with quotes:
   `cd "C:\path with spaces\project" && ls -la`

### Files to create (only under `<project-path>/doc/`)

| File | Purpose |
|------|---------|
| `doc/README.md` | Index, quick start, links |
| `doc/architecture.md` | Components, diagrams, data flow |
| `doc/development.md` | Setup, build, test, troubleshooting |
| `doc/api-reference.md` | Public HTTP/CLI/library surface |

## Phase 0 — Resolve scope

1. Confirm `<project-path>` from `$target-path` / `$ARGUMENTS` (required when documenting an external project). Prefer resolving a relative path against the current working directory to an absolute path before any Write.
2. Set `<output-folder>` to **`doc`** (never use `$output-dir` or any other value).
3. Compute `<absolute-output-path> = <project-path>/doc/`.
4. **Access gate (mandatory):** list the target project root (e.g. `ls` / Glob / Read of its README or `build.gradle` / `package.json`). If the target is not readable, **STOP immediately**. Tell the user to run `/add-dir` with a relative path. Do **not** write files. Do **not** fall back to documenting the current toolkit repo. Do **not** invent content from the project name.
5. **Wrong-target guard:** `${CLAUDE_PROJECT_DIR}` is this documentation toolkit. Only write under it if `<project-path>` resolves to that same directory on purpose. When `$ARGUMENTS` points at another project (e.g. `..\..\Civia\rag-civia-mandate-service`), writing under `${CLAUDE_PROJECT_DIR}/doc/` is a **failure**.
6. Read templates from `${CLAUDE_PROJECT_DIR}/.claude/skills/document-project/templates/`.
7. Read checklist from `${CLAUDE_PROJECT_DIR}/.claude/skills/document-project/checklist.md`.

## Phase 1 — Discovery (read only, thorough)

Explore the target project systematically. Prefer **breadth then depth** — map the tree, then open the important files.

1. List the root directory and note top-level files and folders.
2. Read the existing README, LICENSE, and any existing `doc/` content.
3. Identify the stack from manifest files (`package.json`, `pyproject.toml`, `go.mod`, `Cargo.toml`, `Gemfile`, `pom.xml`, `build.gradle`, etc.).
4. Locate entry points: `main`, `index`, server bootstrap, CLI entry, `app.py`, `main.go`, etc.
5. Map major directories and their responsibilities.
6. Find how to install, build, test, and run (scripts, Makefile, CI workflows).
7. Identify public interfaces: HTTP routes, GraphQL, gRPC, CLI subcommands, exported packages/modules — **open each controller/router file**.
8. Note external services: databases, queues, caches, third-party APIs (from config and code).
9. **Deep pass (required for non-trivial apps):**
   - Read each public controller/handler and its request/response DTOs
   - Read primary service/facade/coordinator classes and trace calls to repositories and external clients
   - Read persistence models and repository interfaces
   - Read security / auth configuration
   - Read `application*.yml` / `.env.example` / Compose / K8s manifests when present
   - Skim tests for behavioral clues when source is ambiguous

Use Grep and Glob to find patterns (`router`, `Route`, `@app`, `export`, `public class`, `@RestController`, etc.) appropriate to the stack.

Record a short **discovery notes** section (for yourself) listing key files inspected — aim for **dozens of files** on a medium service, not 3–5.

## Phase 2 — Analysis (deep)

From discovery, determine and **write down** (for use in Phase 3):

- **Purpose** — What the software does (one rich paragraph, grounded in README and code).
- **Architecture** — Major components, layering, and how they interact (enough to fill the detailed component sections).
- **Data flow** — At least one primary and one secondary request/job lifecycle with steps and failure points.
- **Data model** — Main entities/collections and relationships.
- **Dependencies** — Runtime integrations with config locations.
- **Configuration** — Env vars / profile keys with purpose.
- **Security** — AuthN/AuthZ mechanisms and where enforced.
- **Deployment** — Docker, K8s, serverless, or manual deploy if evidenced.
- **Design decisions** — Patterns visible in code (facades, strategy, async, multi-provider AI, etc.).

If the project is a library, focus on public API and usage patterns. If it is a CLI, document commands and flags. If it is a web app, document routes and auth in depth.

## Phase 3 — Write documentation (required, deep)

**Use the Write tool for each file below.** Create `<absolute-output-path>` if needed (Write creates parent directories).

**Do not write to `<project-path>/` directly.** Only write inside `<project-path>/doc/`.

Replace every template placeholder with real content. Expand sections — do not leave bullet stubs when evidence exists.

### `<absolute-output-path>/README.md`

Use `templates/README-template.md`. Include purpose paragraph, stack table with evidence, capability table, repo overview diagram, and quick start.

### `<absolute-output-path>/architecture.md`

Use `templates/architecture-template.md`. Must include:

- System context + context diagram
- Layer/package overview + component diagram
- Detailed subsection per major component (collaborators, I/O, failures, evidence)
- At least one primary sequence flow (+ secondary when present)
- Data model section (table + ER diagram when relationships are clear)
- Cross-cutting concerns, dependencies, configuration, deployment, design decisions, gaps

### `<absolute-output-path>/development.md`

Use `templates/development-template.md`. Must include prerequisites, env/profiles, build/run/test, layout diagram, where-to-change map, Docker/Compose/K8s when present, CI, debugging, troubleshooting.

### `<absolute-output-path>/api-reference.md`

Use `templates/api-reference-template.md`. Must include auth section, full endpoint index, and a **detailed section per endpoint** (params, schemas from DTOs, responses, errors). Table-only API docs are insufficient.

## Phase 3.5 — Verify files on disk

Before Phase 4, confirm all four required files exist **under `doc/`**:

1. Use Glob on `<absolute-output-path>/**/*.md` — must include `README.md`, `architecture.md`, `development.md`, and `api-reference.md` (additional files such as `modules.md` are allowed).
2. Use Read on each of the four required files to confirm non-empty content.
3. Confirm **no** new documentation `.md` files were written to `<project-path>/` (root). The project README may already exist; do not overwrite or duplicate it with generated docs.
4. If any required file is missing, empty, or written to the wrong location, fix it now. **Do not finish until all four required files exist under `doc/`.**

## Phase 4 — Quality check

Before finishing, verify every item in `checklist.md` in this skill directory. Fix any failures with Write/Edit.

## Phase 5 — Report to user

Respond with:

1. **Summary** — What was documented and for which project path.
2. **Files written** — Full absolute paths under `<project-path>/doc/` (must list all four).
3. **Highlights** — 3–5 key architectural or usage findings (brief; details belong in the files).
4. **Gaps** — Anything you could not determine from the codebase.
5. **Suggested next steps** — Recommend `/document-modules <project-path>` to produce `doc/modules.md` (functional/technical module boundaries). Optionally mention ADRs or runbooks if relevant.

## Constraints

- Do not modify application source code unless the user explicitly asked to fix code while documenting.
- Do not copy large blocks of source into docs; summarize and link to files.
- Do not document credentials, tokens, or private keys.
- Do not write documentation outside `<project-path>/doc/`.
- Do not write under the documentation toolkit's `doc/` when the target is a different project.
- If the target project cannot be read, abort with no file writes.
- Prefer updating existing pages under `doc/` over duplicating content in the project README.
