---
name: document-modules
description: Create a module decomposition document (doc/modules.md) from existing codebase-doc-writer documentation and the target source tree. Use when the user asks for module boundaries, functional/technical modules, or runs /document-modules.
when_to_use: User says "document modules", "module boundaries", "decompose into modules", or wants doc/modules.md after /document-project.
argument-hint: [target-project-path]
arguments:
  - target-path
disable-model-invocation: true
context: fork
agent: module-doc-writer
allowed-tools: Read Grep Glob Write Edit Bash
---

# Document project modules

You decompose an already-documented software project into **self-contained modules** with **unique responsibilities** and the **least inter-module dependencies**. Document each module from a **functional** and a **technical** perspective.

Follow this workflow exactly. Do not invent modules without evidence from existing `doc/` files and the source tree.

**Division of responsibility:** This skill owns the workflow, paths, templates, checklist, and report. The forked agent `module-doc-writer` supplies persona, tools, and model — it must execute *this* prompt, not a parallel copy of these steps.

## Critical — depth and diagrams (mandatory)

Shallow module lists are a **failed** run.

1. Each module needs full **functional** and **technical** sections, an **internal sequence diagram**, failure modes, and evidence — not just a one-line responsibility.
2. Include at least: module map table, dependency diagram, responsibility-boundaries diagram, and per-module internal flow diagrams for major modules.
3. Add a **coupling analysis** table explaining dependency strength and whether edges can be reduced.
4. Re-verify against source; deepen beyond what `architecture.md` already says — do not merely copy component names into a table.

## Critical — file output is mandatory

**This task is NOT complete until `doc/modules.md` exists on disk.**

- You **MUST** use the **Write** tool to create `<project-path>/doc/modules.md`.
- Do **NOT** paste the full modules document in chat instead of writing the file.
- Phase 5 is a short report only — the deliverable is the file.

## Critical — output location and filename (fixed)

- Output file is **always** `<project-path>/doc/modules.md`
- Filename is **`modules.md`** (not `MODULES.md`, `module.md`, or any other name)
- Folder is **always** `doc/` (same folder used by `codebase-doc-writer`)
- **Never** write module documentation to the project root

## Target

- **Project path:** `$target-path` (if empty, use the repository root or the path from `/add-dir`)
- **Absolute output path:** `<project-path>/doc/modules.md`
- **Arguments (raw):** $ARGUMENTS

### Path rules

1. Prefer **relative paths on Windows** for `/add-dir` and `/document-modules`.
2. Example from this toolkit repo:
   `/document-modules ..\..\Civia\rag-civia-mandate-service`
3. Ensure the target is accessible via `/add-dir` if outside the workspace.
4. Use **absolute paths** in Write/Edit calls.

## Phase 0 — Resolve scope and prerequisites

1. Confirm `<project-path>` from `$target-path` / `$ARGUMENTS`. Resolve relative paths to absolute before writing.
2. **Access gate (mandatory):** confirm the target project root is readable. If not, **STOP** and tell the user to `/add-dir` — do not write files; do not fall back to the toolkit repo.
3. Confirm `<project-path>/doc/` exists.
4. **Required inputs** (from `/document-project` / `codebase-doc-writer`):
   - `<project-path>/doc/architecture.md`
   - `<project-path>/doc/README.md`
5. **Preferred inputs:**
   - `<project-path>/doc/api-reference.md`
   - `<project-path>/doc/development.md`
6. If required inputs are missing, **stop** and tell the user to run `/document-project <path>` first.
7. **Wrong-target guard:** do not write `modules.md` under `${CLAUDE_PROJECT_DIR}/doc/` when `$ARGUMENTS` points at another project.
8. Read the template: `${CLAUDE_PROJECT_DIR}/.claude/skills/document-modules/templates/modules-template.md`
9. Read the checklist: `${CLAUDE_PROJECT_DIR}/.claude/skills/document-modules/checklist.md`

## Phase 1 — Ingest existing documentation

Read thoroughly:

1. `doc/architecture.md` — components, data flow, dependencies
2. `doc/README.md` — project purpose and stack
3. `doc/api-reference.md` — public surface (if present)
4. `doc/development.md` — runtime/config clues (if present)

Extract candidate capabilities, components, and integration points. Do not copy architecture prose wholesale into `modules.md`; reorganize into module boundaries.

## Phase 2 — Verify against the codebase

Ground every candidate module in source:

1. Map packages/directories to candidate modules
2. Identify entry points, controllers, services, repositories, clients
3. Trace imports/calls that imply dependencies between candidates
4. Adjust boundaries when docs and code disagree — **prefer code evidence**
5. Record evidence paths for each module

## Phase 3 — Decompose into modules

Apply these rules strictly:

1. **Unique responsibility** — each module owns one capability; no shared ownership of the same concern
2. **Self-contained** — each module has clear inputs, outputs, and owned internals
3. **Least dependencies** — minimize edges between modules; prefer depending on contracts/interfaces; avoid cycles
4. **Functional + technical** — every module section must include both perspectives in depth
5. **Granularity** — prefer a small set of cohesive modules over a package-by-package inventory
6. **Depth** — each module must include internal flow (mermaid sequence), failure modes, and evidence; shallow bullet-only modules fail the checklist

Produce:

- A module map table (with key paths)
- Dependency diagram + responsibility-boundaries diagram
- One full deep section per module (functional, technical, internal flow, dependencies, failures, evidence)
- Coupling analysis table
- Cross-cutting concerns (only if they are not full modules)
- Gaps / TBD items

## Phase 4 — Write `doc/modules.md` (required)

1. Use the **Write** tool to create `<project-path>/doc/modules.md`
2. Follow `templates/modules-template.md` structure; replace every placeholder
3. **Edit** `<project-path>/doc/README.md` to add this index row if missing:

   `| [Modules](./modules.md) | Functional and technical module boundaries |`

## Phase 4.5 — Verify on disk

1. Glob/Read `<project-path>/doc/modules.md` — must exist and be non-empty
2. Confirm the file is under `doc/` and named exactly `modules.md`
3. Confirm `doc/README.md` links to `./modules.md`
4. If anything is wrong, fix it before finishing

## Phase 5 — Quality check

Verify every item in `checklist.md` in this skill directory. Fix failures with Write/Edit.

## Phase 6 — Report to user

Respond with:

1. **Summary** — Module decomposition for which project path
2. **Files written** — Absolute path to `doc/modules.md` (and note if `doc/README.md` was updated)
3. **Module list** — IDs and one-line responsibilities
4. **Dependency highlights** — How coupling was minimized; any cycles/debt
5. **Gaps** — Anything not determined from docs/code

## Constraints

- Do not modify application source code
- Do not write documentation outside `<project-path>/doc/`
- Do not invent modules, APIs, or dependencies without evidence
- Do not replace or delete `architecture.md` / other existing docs — only add `modules.md` and update the README index link
