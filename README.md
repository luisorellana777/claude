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

### 2. Local model with Ollama (recommended)

This is the simplest setup for running Claude Code against a local coding model.

#### Install Ollama

Download and install from [Ollama](https://ollama.com).

Verify the server is running:

```bash
ollama serve
```

#### Download a coding model

For an RTX 5090, one of these models is a good fit:

| Model | Recommendation |
|-------|----------------|
| Qwen3-Coder 30B | Best overall balance |
| DeepSeek-Coder V2 | Excellent |
| Qwen2.5-Coder 32B | Very good |
| GPT-OSS-20B | Fast and capable |

Example:

```bash
ollama pull qwen3-coder
```

#### Configure Claude Code

On Windows PowerShell:

```powershell
[Environment]::SetEnvironmentVariable(
    "ANTHROPIC_BASE_URL",
    "http://localhost:11434",
    "User"
)

[Environment]::SetEnvironmentVariable(
    "ANTHROPIC_AUTH_TOKEN",
    "ollama",
    "User"
)

[Environment]::SetEnvironmentVariable(
    "ANTHROPIC_MODEL",
    "qwen3-coder",
    "User"
)
```

Restart your terminal after setting environment variables, then run:

```bash
claude
```

### 3. Run from this repository

```bash
cd /path/to/this-repo
claude
```

### 4. Point at the project to document

Inside Claude Code (quote paths that contain spaces):

```text
/add-dir "/path/to/target-software"
/document-project "/path/to/target-software"
```

Or with a custom output directory:

```text
/document-project "/path/to/target-software" custom-docs
```

**Example** (Windows path with spaces and custom output folder):

```text
/add-dir "<PATH-TO-YOUR-PROJECT>\rag-civia-mandate-service"
/document-project "<PATH-TO-YOUR-PROJECT>\rag-civia-mandate-service" civia-documentation
```

This writes documentation to:

```
<PATH-TO-YOUR-PROJECT>\rag-civia-mandate-service\civia-documentation\
```

The agent writes files to `<target-software>/custom-docs/` (or `docs/` if omitted). Verify the **Files written** list in the response — chat-only summaries mean the run did not finish.

### 5. Alternative — install into the target repo

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
