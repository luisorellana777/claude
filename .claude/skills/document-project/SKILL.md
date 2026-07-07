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

## Target

- **Project path:** `$target-path` (if empty, use the repository root or the path from `/add-dir`)
- **Output directory:** `$output-dir` (if empty, use `docs/` under the project path)
- **Arguments (raw):** $ARGUMENTS

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

## Phase 3 — Write documentation

Create the output directory if it does not exist. Write these files unless the user requested a subset:

### `docs/README.md`

Use structure from `templates/README-template.md`. Include:

- Project name and one-line description
- Link to architecture, development, and API docs
- Quick start (minimal steps to run locally)
- Table of contents

### `docs/architecture.md`

Use structure from `templates/architecture-template.md`. Include:

- System context (what talks to what)
- Component breakdown with file/directory references
- Mermaid diagram(s) for architecture and/or main request flow
- Key design decisions visible in code (routing, layering, patterns)
- External dependencies and integrations

### `docs/development.md`

Use structure from `templates/development-template.md`. Include:

- Prerequisites (language version, tools)
- Clone and install steps from real project files
- Build, test, lint commands from package scripts or CI
- Environment variables (name, purpose, default if documented)
- Common troubleshooting tied to actual project setup

### `docs/api-reference.md`

Use structure from `templates/api-reference-template.md`. Include:

- HTTP endpoints, CLI commands, or exported symbols — whichever is the public surface
- For each: method/path or signature, purpose, parameters, response or return type
- Link to source files defining each interface

Read templates from this skill's `templates/` folder for section headings; replace every placeholder with content from the codebase.

## Phase 4 — Quality check

Before finishing, verify every item in `checklist.md` in this skill directory. Fix any failures.

## Phase 5 — Report to user

Respond with:

1. **Summary** — What was documented and for which project path.
2. **Files written** — List of paths created or updated.
3. **Highlights** — 3–5 key architectural or usage findings.
4. **Gaps** — Anything you could not determine from the codebase.
5. **Suggested next steps** — Optional deeper docs (ADRs, runbooks) if relevant.

## Constraints

- Do not modify application source code unless the user explicitly asked to fix code while documenting.
- Do not copy large blocks of source into docs; summarize and link to files.
- Do not document credentials, tokens, or private keys.
- Prefer updating existing `docs/` pages over duplicating content in README unless README is the agreed output.
