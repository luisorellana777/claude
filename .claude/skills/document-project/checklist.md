# Documentation quality checklist

Complete this checklist before marking a documentation run as finished.

## Discovery

- [ ] Read the project README (if present)
- [ ] Identified language(s) and framework(s) from manifest or config files
- [ ] Located entry point(s) and main modules
- [ ] Found install, build, and test commands from scripts or CI
- [ ] Checked for existing documentation to align with or extend

## Accuracy

- [ ] No APIs, endpoints, or modules documented without source evidence
- [ ] Commands in docs match `package.json` scripts, Makefile targets, or equivalent
- [ ] Environment variable names match config/code exactly
- [ ] Version requirements match stated engine/python/go/rust versions in project files
- [ ] Uncertain items marked as `Not found in codebase` or `TBD`

## Completeness (default set)

All files must be under `<project-path>/doc/` (fixed output folder).

- [ ] `doc/README.md` exists on disk with index and quick start (verified with Read)
- [ ] `doc/architecture.md` exists on disk with component overview
- [ ] `doc/development.md` exists on disk with setup and test instructions
- [ ] `doc/api-reference.md` exists on disk or N/A is explained (e.g., internal-only app)
- [ ] All four files were created with the Write tool (not chat-only summaries)
- [ ] No documentation files were written to the project root (only under `doc/`)

## Quality

- [ ] Pages cross-link with relative Markdown links
- [ ] At least one mermaid diagram in architecture doc (if system has multiple components)
- [ ] Headings create a clear hierarchy
- [ ] No marketing fluff or unsupported superlatives
- [ ] Code examples use the project's language and style

## Safety

- [ ] No secrets, API keys, or credentials in generated docs
- [ ] No large verbatim copies of proprietary or generated boilerplate without purpose
