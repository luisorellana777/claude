---
name: document-project
description: Analyze a software codebase and generate technical documentation (architecture, development guide, API reference). Use when the user asks to document a project, create docs for a repository, or run the documentation workflow.
when_to_use: User says "document this project", "create technical documentation", "write docs for this repo", or provides a path to a codebase to document.
argument-hint: [target-project-path] [output-dir]
arguments:
  - target-path
  - output-dir
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

## Target

Resolve paths before discovery:

- **Project path:** `$target-path` (if empty, use the repository root or the path from `/add-dir`)
- **Output folder name:** `$output-dir` (if empty, use `docs`)
- **Absolute output path:** `<project-path>/<output-folder>/`
- **Arguments (raw):** $ARGUMENTS

### Path rules

1. **Quote paths with spaces** (common on Windows). Example:
   `"C:\Users\...\Data Science\...\rag-civia-mandate-service"`
2. When the target project is **outside** the current working directory, ensure it is accessible. If Write fails or the path is not readable, tell the user to run `/add-dir "<project-path>"` and retry.
3. Use **absolute paths** in every Write call, e.g.:
   `"<project-path>/<output-folder>/README.md"`
4. For Bash on Windows paths with spaces, always `cd` with quotes:
   `cd "C:\path with spaces\project" && ls -la`

### Files to create (under absolute output path)

| File | Purpose |
|------|---------|
| `README.md` | Index, quick start, links |
| `architecture.md` | Components, diagrams, data flow |
| `development.md` | Setup, build, test, troubleshooting |
| `api-reference.md` | Public HTTP/CLI/library surface |

## Phase 0 — Resolve scope

1. Confirm `<project-path>` from `$target-path` or `/add-dir`.
2. Confirm `<output-folder>` from `$output-dir` or default `docs`.
3. Compute `<absolute-output-path> = <project-path>/<output-folder>/`.
4. Read templates from `${CLAUDE_PROJECT_DIR}/.claude/skills/document-project/templates/`.
5. Read checklist from `${CLAUDE_PROJECT_DIR}/.claude/skills/document-project/checklist.md`.

## Phase 1 — Discovery (read only)

Explore the target project systematically:

1. List the root directory and note top-level files and folders.
2. Read the existing README, LICENSE, and any `docs/` or `doc/` content.
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

Before Phase 4, confirm all four files exist:

1. Use Glob on `<absolute-output-path>/**/*.md`.
2. Use Read on each file to confirm non-empty content.
3. If any file is missing or empty, write it now. **Do not finish until all four exist.**

## Phase 4 — Quality check

Before finishing, verify every item in `checklist.md` in this skill directory (treating `<output-folder>` as the docs root). Fix any failures with Write/Edit.

## Phase 5 — Report to user

Respond with:

1. **Summary** — What was documented and for which project path.
2. **Files written** — Full absolute paths of every file created or updated (must list all four).
3. **Highlights** — 3–5 key architectural or usage findings (brief; details belong in the files).
4. **Gaps** — Anything you could not determine from the codebase.
5. **Suggested next steps** — Optional deeper docs (ADRs, runbooks) if relevant.

## Constraints

- Do not modify application source code unless the user explicitly asked to fix code while documenting.
- Do not copy large blocks of source into docs; summarize and link to files.
- Do not document credentials, tokens, or private keys.
- Prefer updating existing pages under `<output-folder>` over duplicating content in the project README unless README is the agreed output.
