# Documentation quality checklist

Complete this checklist before marking a documentation run as finished.

## Discovery

- [ ] Read the project README (if present)
- [ ] Identified language(s) and framework(s) from manifest or config files
- [ ] Located entry point(s) and main modules
- [ ] Found install, build, and test commands from scripts or CI
- [ ] Checked for existing documentation to align with or extend
- [ ] Deep pass: opened controllers/routers, primary services, repositories/models, security config, and app config (not just root files)

## Accuracy

- [ ] No APIs, endpoints, or modules documented without source evidence
- [ ] Commands in docs match `package.json` scripts, Makefile targets, or equivalent
- [ ] Environment variable names match config/code exactly
- [ ] Version requirements match stated engine/python/go/rust versions in project files
- [ ] Uncertain items marked as `Not found in codebase` or `TBD`
- [ ] No invented/generic boilerplate when the target codebase was readable

## Completeness (default set)

All files must be under `<project-path>/doc/` (fixed output folder).

- [ ] `doc/README.md` exists on disk with index, purpose, stack table, and quick start
- [ ] `doc/architecture.md` exists with detailed components, data model, and design decisions
- [ ] `doc/development.md` exists with setup, layout, where-to-change, and troubleshooting
- [ ] `doc/api-reference.md` exists with per-endpoint detail (or N/A explained)
- [ ] All four files were created with the Write tool (not chat-only summaries)
- [ ] No documentation files were written to the project root (only under `doc/`)
- [ ] Files were written under the **target** project path from `$ARGUMENTS`, not under the documentation toolkit unless that was the intentional target
- [ ] If the target was inaccessible, the run aborted with no writes (did not invent content)

## Depth and diagrams

- [ ] `architecture.md` has **≥ 3** mermaid diagrams (context, component/layer, sequence; plus ER/deploy when applicable)
- [ ] Each major component has collaborators, I/O, failure modes, and evidence — not a one-liner
- [ ] At least one primary flow is narrated step-by-step with a sequence diagram
- [ ] `api-reference.md` documents each public endpoint in its own section (params + response schema from DTOs)
- [ ] `development.md` includes env/profiles and a where-to-change map
- [ ] Docs are substantive for the size of the codebase (shallow one-pagers fail for multi-package services)

## Quality

- [ ] Pages cross-link with relative Markdown links
- [ ] Headings create a clear hierarchy
- [ ] No marketing fluff or unsupported superlatives
- [ ] Code examples use the project's language and style

## Safety

- [ ] No secrets, API keys, or credentials in generated docs
- [ ] No large verbatim copies of proprietary or generated boilerplate without purpose
