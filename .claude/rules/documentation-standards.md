---
paths:
  - "doc/**"
  - "**/doc/**"
---

# Documentation standards

Apply these rules when writing or editing generated documentation.

## Voice and structure

- Use present tense and active voice ("The service validates tokens" not "Tokens will be validated").
- Start each page with a one-sentence summary of what the reader will learn, then go deep.
- Use headings to create a scannable hierarchy; avoid walls of text without structure.
- Prefer tables for comparing options, environment variables, or endpoints.
- Use **multiple** mermaid diagrams for architecture, sequences, module boundaries, and data relationships when they clarify the system.
- Depth over brevity: onboarding engineers should understand flows, collaborators, and failure modes from the docs alone.

## Depth requirements

- Architecture docs must cover components in detail (not a short bullet list), with context, layer, and sequence diagrams at minimum.
- API docs must detail each public endpoint (parameters, schemas, responses, errors), not only an index table.
- Development docs must include env/profiles, layout, where-to-change guidance, and troubleshooting grounded in the repo.
- Module docs must include functional + technical views, internal flows, and coupling analysis.
- Shallow or generic documentation is unacceptable when the source is available.

## Accuracy

- Never document features, endpoints, or config keys you did not find in the codebase.
- Quote default values and env var names exactly as defined in source or config files.
- Distinguish **implemented** behavior from **planned** or **deprecated** code (check comments, `@deprecated`, README notes).
- If behavior is unclear, say so and point to the relevant source files for the reader to verify.

## Code examples

- Examples must match the project's language and conventions.
- Prefer snippets taken from or aligned with real project code (DTOs, signatures, commands).
- Include import paths and minimal context so examples are copy-pasteable where possible.
- Show example commands for install, build, and test using scripts from `package.json`, `Makefile`, `pyproject.toml`, etc.

## File naming and layout

- All generated documentation lives under `doc/` at the target project root — never in the project root itself.
- Use lowercase kebab-case for doc filenames: `architecture.md`, `api-reference.md`, `modules.md`.
- Cross-link between docs with relative Markdown links.
- Keep `doc/README.md` as the entry point with links to all other pages.
- Module decomposition belongs in `doc/modules.md` only (produced by `/document-modules`).

## What to avoid

- Writing documentation files to the project root (e.g. `ARCHITECTURE.md`, `DEVELOPMENT.md` at repo root).
- Marketing language or unsubstantiated claims ("blazing fast", "enterprise-grade").
- One-page "overview" docs that omit flows, schemas, and evidence for a non-trivial codebase.
- Duplicating auto-generated API docs unless you are curating and explaining them.
- Inventing architecture when the target project is inaccessible — abort instead.
