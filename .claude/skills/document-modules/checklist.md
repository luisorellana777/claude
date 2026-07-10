# Module documentation quality checklist

Complete this checklist before marking a `/document-modules` run as finished.

## Prerequisites

- [ ] `doc/architecture.md` exists and was read
- [ ] `doc/README.md` exists and was read
- [ ] `doc/api-reference.md` was read if present
- [ ] Candidate modules were verified against source paths/symbols

## Output

- [ ] `doc/modules.md` exists on disk (exact filename)
- [ ] File was created with the Write tool (not chat-only)
- [ ] No module documentation was written to the project root
- [ ] `doc/README.md` links to `./modules.md`
- [ ] Written under the **target** project, not the documentation toolkit (unless intentional)

## Module quality

- [ ] Each module has a unique responsibility (no overlapping ownership)
- [ ] Each module includes a deep **functional** perspective (capability, actors, inputs/outputs, behavior)
- [ ] Each module includes a deep **technical** perspective (location, interfaces, data, config, async)
- [ ] Each major module includes an **internal sequence diagram** and narrative
- [ ] Failure modes / edge cases documented per module
- [ ] Inter-module dependencies are listed and minimized
- [ ] Dependency diagram + responsibility-boundaries diagram present
- [ ] Coupling analysis table present
- [ ] Evidence table cites docs and/or source paths for each module
- [ ] Uncertain items marked `Not found in codebase` or `TBD`
- [ ] Not a shallow rename of architecture components into a table

## Safety

- [ ] No secrets, API keys, or credentials in `modules.md`
- [ ] No large verbatim copies of proprietary source without purpose
