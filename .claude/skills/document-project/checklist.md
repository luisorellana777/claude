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

- [ ] `docs/README.md` exists with index and quick start
- [ ] `docs/architecture.md` exists with component overview
- [ ] `docs/development.md` exists with setup and test instructions
- [ ] `docs/api-reference.md` exists or N/A is explained (e.g., internal-only app)

## Quality

- [ ] Pages cross-link with relative Markdown links
- [ ] At least one mermaid diagram in architecture doc (if system has multiple components)
- [ ] Headings create a clear hierarchy
- [ ] No marketing fluff or unsupported superlatives
- [ ] Code examples use the project's language and style

## Safety

- [ ] No secrets, API keys, or credentials in generated docs
- [ ] No large verbatim copies of proprietary or generated boilerplate without purpose
