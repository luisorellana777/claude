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

1. Confirm `<project-path>` from `$target-path` or `/add-dir`.
2. Set `<output-folder>` to **`doc`** (never use `$output-dir` or any other value).
3. Compute `<absolute-output-path> = <project-path>/doc/`.
4. Read templates from `${CLAUDE_PROJECT_DIR}/.claude/skills/document-project/templates/`.
5. Read checklist from `${CLAUDE_PROJECT_DIR}/.claude/skills/document-project/checklist.md`.

## Phase 1 — Discovery (read only)

Explore the target project systematically:

1. List the root directory and note top-level files and folders.
2. Read the existing README, LICENSE, and any existing `doc/` content.
3. Identify the stack from manifest files (`package.json`, `pyproject.toml`, `go.mod`, `Cargo.toml`, `Gemfile`, `pom.xml`, `build.gradle`, etc.).
4. Locate entry points: `main`, `index`, server bootstrap, CLI entry, `app.py`, `main.go`, etc.
5. Map major directories and their responsibilities.
6. Find how to install, build, test, and run (scripts, Makefile, CI workflows).
7. Identify public interfaces: HTTP routes, GraphQL, gRPC, CLI subcommands, exported packages/modules.
8. Note external services: databases, queues, caches, third-party APIs (from config and code).

Use Grep and Glob to find patterns (`router`, `Route`, `@app`, `export`, `public class`, etc.) appropriate to the stack.

Record a short **discovery notes** section (for yourself) listing key files inspected.

## Phase 2 — Analysis

From discovery, determine:

- **Purpose** — What the software does (one paragraph, grounded in README and code).
- **Architecture** — Major components and how they interact.
- **Data flow** — Request/job lifecycle or main processing pipeline.
- **Dependencies** — Runtime and dev dependencies worth calling out.
- **Configuration** — Env vars, config files, secrets handling (without exposing secret values).
- **Deployment** — Docker, K8s, serverless, or manual deploy if evidenced in repo.

If the project is a library, focus on public API and usage patterns. If it is a CLI, document commands and flags. If it is a web app, document routes and auth if present.

## Phase 3 — Write documentation (required)

**Use the Write tool for each file below.** Create `<absolute-output-path>` if needed (Write creates parent directories).

**Do not write to `<project-path>/` directly.** Only write inside `<project-path>/doc/`.

### `<absolute-output-path>/README.md`

Use structure from `templates/README-template.md`. Include:

- Project name and one-line description
- Link to architecture, development, and API docs
- Quick start (minimal steps to run locally)
- Table of contents

### `<absolute-output-path>/architecture.md`

Use structure from `templates/architecture-template.md`. Include:

- System context (what talks to what)
- Component breakdown with file/directory references
- Mermaid diagram(s) for architecture and/or main request flow
- Key design decisions visible in code (routing, layering, patterns)
- External dependencies and integrations

### `<absolute-output-path>/development.md`

Use structure from `templates/development-template.md`. Include:

- Prerequisites (language version, tools)
- Clone and install steps from real project files
- Build, test, lint commands from package scripts or CI
- Environment variables (name, purpose, default if documented)
- Common troubleshooting tied to actual project setup

### `<absolute-output-path>/api-reference.md`

Use structure from `templates/api-reference-template.md`. Include:

- HTTP endpoints, CLI commands, or exported symbols — whichever is the public surface
- For each: method/path or signature, purpose, parameters, response or return type
- Link to source files defining each interface

Read templates from this skill's `templates/` folder for section headings; replace every placeholder with content from the codebase.

## Phase 3.5 — Verify files on disk

Before Phase 4, confirm all four files exist **only under `doc/`**:

1. Use Glob on `<absolute-output-path>/**/*.md` — must find exactly the four files.
2. Use Read on each file to confirm non-empty content.
3. Confirm **no** new documentation `.md` files were written to `<project-path>/` (root). The project README may already exist; do not overwrite or duplicate it with generated docs.
4. If any file is missing, empty, or written to the wrong location, fix it now. **Do not finish until all four exist under `doc/`.**

## Phase 4 — Quality check

Before finishing, verify every item in `checklist.md` in this skill directory. Fix any failures with Write/Edit.

## Phase 5 — Report to user

Respond with:

1. **Summary** — What was documented and for which project path.
2. **Files written** — Full absolute paths under `<project-path>/doc/` (must list all four).
3. **Highlights** — 3–5 key architectural or usage findings (brief; details belong in the files).
4. **Gaps** — Anything you could not determine from the codebase.
5. **Suggested next steps** — Optional deeper docs (ADRs, runbooks) if relevant.

## Constraints

- Do not modify application source code unless the user explicitly asked to fix code while documenting.
- Do not copy large blocks of source into docs; summarize and link to files.
- Do not document credentials, tokens, or private keys.
- Do not write documentation outside `<project-path>/doc/`.
- Prefer updating existing pages under `doc/` over duplicating content in the project README.
