---
paths:
  - "doc/**"
  - "**/doc/**"
---

# Documentation standards

Apply these rules when writing or editing generated documentation.

## Voice and structure

- Use present tense and active voice ("The service validates tokens" not "Tokens will be validated").
- Start each page with a one-sentence summary of what the reader will learn.
- Use headings to create a scannable hierarchy; avoid walls of text.
- Prefer tables for comparing options, environment variables, or endpoints.
- Use mermaid diagrams for architecture and sequence flows when they clarify relationships.

## Accuracy

- Never document features, endpoints, or config keys you did not find in the codebase.
- Quote default values and env var names exactly as defined in source or config files.
- Distinguish **implemented** behavior from **planned** or **deprecated** code (check comments, `@deprecated`, README notes).
- If behavior is unclear, say so and point to the relevant source files for the reader to verify.

## Code examples

- Examples must match the project's language and conventions.
- Prefer snippets taken from or aligned with real project code.
- Include import paths and minimal context so examples are copy-pasteable where possible.
- Show example commands for install, build, and test using scripts from `package.json`, `Makefile`, `pyproject.toml`, etc.

## File naming and layout

- All generated documentation lives under `doc/` at the target project root — never in the project root itself.
- Use lowercase kebab-case for doc filenames: `architecture.md`, `api-reference.md`.
- Cross-link between docs with relative Markdown links.
- Keep `doc/README.md` as the entry point with links to all other pages.

## What to avoid

- Writing documentation files to the project root (e.g. `ARCHITECTURE.md`, `DEVELOPMENT.md` at repo root).
- Marketing language or unsubstantiated claims ("blazing fast", "enterprise-grade").
- Duplicating auto-generated API docs unless you are curating and explaining them.
- Documenting private/internal implementation details unless the user asked for deep internals.
