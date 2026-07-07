# Software Documentation Agent (Claude Code)

A [Claude Code](https://code.claude.com) project that configures an agent to read a software repository and produce technical documentation grounded in the actual codebase.

## What it does

- Explores a target project (structure, stack, entry points, tests, public APIs)
- Writes a default documentation set under `docs/`
- Enforces accuracy via a discovery-first workflow and quality checklist
- Supports large repos through a dedicated `codebase-doc-writer` subagent

## Project structure

```
.
├── CLAUDE.md                          # Session instructions
├── .claude/
│   ├── settings.json                  # Tool permissions
│   ├── agents/
│   │   └── codebase-doc-writer.md     # Documentation subagent
│   ├── rules/
│   │   └── documentation-standards.md # Writing standards
│   └── skills/
│       └── document-project/
│           ├── SKILL.md               # Main prompt (/document-project)
│           ├── checklist.md           # Quality gate
│           └── templates/             # Output templates
```

## Quick start

### 1. Install Claude Code

Follow the [Claude Code installation guide](https://code.claude.com/docs/en/setup).

### 2. Run from this repository

```bash
cd /path/to/this-repo
claude
```

### 3. Point at the project to document

Inside Claude Code:

```text
/add-dir /path/to/target-software
/document-project /path/to/target-software
```

Or with a custom output directory:

```text
/document-project /path/to/target-software custom-docs
```

### 4. Alternative — install into the target repo

Copy `CLAUDE.md` and `.claude/` into the root of the project you want to document, then:

```bash
cd /path/to/target-software
claude
/document-project
```

## Generated documentation (default)

| File | Contents |
|------|----------|
| `docs/README.md` | Index and quick start |
| `docs/architecture.md` | Components, diagrams, dependencies |
| `docs/development.md` | Setup, build, test, troubleshooting |
| `docs/api-reference.md` | HTTP, CLI, or library public surface |

## Primary prompt

The documentation workflow is defined in:

`.claude/skills/document-project/SKILL.md`

Invoke it with `/document-project`. The skill runs in a forked `codebase-doc-writer` subagent so exploration stays out of your main conversation on large codebases.

## Customization

- Edit templates in `.claude/skills/document-project/templates/` to change output shape
- Adjust `.claude/rules/documentation-standards.md` for team style
- Tune `.claude/agents/codebase-doc-writer.md` for model or tool restrictions

## License

Use and adapt this project freely for documenting your own software.
