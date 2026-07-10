# Software Documentation Agent (Claude Code)

A [Claude Code](https://code.claude.com) project that configures an agent to read a software repository and produce technical documentation grounded in the actual codebase.

## What it does

- Explores a target project (structure, stack, entry points, tests, public APIs)
- Writes a default documentation set under `doc/` in the target project
- Optionally decomposes the system into self-contained modules (`doc/modules.md`)
- Enforces accuracy via a discovery-first workflow and quality checklist
- Supports large repos through dedicated `codebase-doc-writer` and `module-doc-writer` subagents

## Project structure

```
.
├── CLAUDE.md                          # Session instructions
├── .claude/
│   ├── settings.json                  # Tool permissions
│   ├── agents/
│   │   ├── codebase-doc-writer.md     # Persona/tools for /document-project
│   │   └── module-doc-writer.md       # Persona/tools for /document-modules
│   ├── rules/
│   │   └── documentation-standards.md # Writing standards
│   └── skills/
│       ├── document-project/
│       │   ├── SKILL.md               # /document-project
│       │   ├── checklist.md
│       │   └── templates/
│       └── document-modules/
│           ├── SKILL.md               # /document-modules
│           ├── checklist.md
│           └── templates/
│               └── modules-template.md
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

Inside Claude Code:

```text
/add-dir ../path/to/target-software
/document-project ../path/to/target-software
```

**Example** (from this repo, using a relative path — recommended on Windows):

```text
/add-dir ..\..\Civia\rag-civia-mandate-service
/document-project ..\..\Civia\rag-civia-mandate-service
```

This writes documentation to:

```
..\..\Civia\rag-civia-mandate-service\doc\
├── README.md
├── architecture.md
├── development.md
└── api-reference.md
```

All generated documentation **must** go in `doc/` — never in the project root or a custom folder.

### 5. Document modules (after base docs exist)

`/document-modules` reads the docs produced by `/document-project` (plus the source tree) and writes a separate module decomposition file:

```text
/document-modules ..\..\Civia\rag-civia-mandate-service
```

This adds:

```
..\..\Civia\rag-civia-mandate-service\doc\modules.md
```

Each module is documented from a **functional** and a **technical** perspective, with unique responsibility and minimal dependencies on other modules.

#### Windows: `/add-dir` and paths with spaces

Claude Code can mis-parse **quoted absolute paths** in `/add-dir`, concatenating them to the current directory (e.g. `...\claude"C:\Users\...`). Use one of these instead:

| Method | Command |
|--------|---------|
| **Relative path (recommended)** | `/add-dir ..\..\Civia\rag-civia-mandate-service` |
| **At startup** | `claude --add-dir "..\..\Civia\rag-civia-mandate-service"` |
| **Settings (persistent)** | Add `"../../Civia/rag-civia-mandate-service"` to `permissions.additionalDirectories` in `.claude/settings.json` |

For `/document-project` and `/document-modules`, relative paths work the same way. Only use quoted absolute paths if your Claude Code version handles them correctly.

The agents write files to `<target-software>/doc/` only. Verify the **Files written** list in the response — chat-only summaries mean the run did not finish.

### 6. Alternative — install into the target repo

Copy `CLAUDE.md` and `.claude/` into the root of the project you want to document, then:

```bash
cd /path/to/target-software
claude
/document-project
/document-modules
```

## Generated documentation

All files are written to `doc/` under the target project:

| File | Contents | Produced by |
|------|----------|-------------|
| `doc/README.md` | Index and quick start | `/document-project` |
| `doc/architecture.md` | Components, diagrams, dependencies | `/document-project` |
| `doc/development.md` | Setup, build, test, troubleshooting | `/document-project` |
| `doc/api-reference.md` | HTTP, CLI, or library public surface | `/document-project` |
| `doc/modules.md` | Functional + technical module boundaries | `/document-modules` |

## Primary prompts

| Command | Skill | Agent |
|---------|-------|-------|
| `/document-project` | `.claude/skills/document-project/SKILL.md` | `codebase-doc-writer` |
| `/document-modules` | `.claude/skills/document-modules/SKILL.md` | `module-doc-writer` |

Both skills run in a forked subagent so exploration stays out of your main conversation on large codebases. Run `/document-project` before `/document-modules`.

## Customization

- Edit templates in `.claude/skills/document-project/templates/` and `.claude/skills/document-modules/templates/` to change output shape
- Adjust `.claude/rules/documentation-standards.md` for team style
- Tune `.claude/agents/*.md` for model or tool restrictions (workflows live in the matching `SKILL.md`)

## License

Use and adapt this project freely for documenting your own software.
