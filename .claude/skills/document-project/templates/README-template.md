# {{PROJECT_NAME}}

{{ONE_LINE_DESCRIPTION}}

## Documentation index

| Document | Description |
|----------|-------------|
| [Architecture](./architecture.md) | Deep system design, components, flows, and diagrams |
| [Development](./development.md) | Local setup, build, test, layout, and troubleshooting |
| [API reference](./api-reference.md) | Full public HTTP/CLI/library surface |
| [Modules](./modules.md) | Functional and technical module boundaries (after `/document-modules`) |

## What this system does

{{PURPOSE_PARAGRAPH}}

## Quick start

1. Prerequisites: {{PREREQUISITES}}
2. Install: `{{INSTALL_COMMAND}}`
3. Run: `{{RUN_COMMAND}}`
4. Test: `{{TEST_COMMAND}}`

See [Development](./development.md) for profiles, Docker/Compose, and debugging.

## Repository overview

| Path | Purpose | Notable contents |
|------|---------|------------------|
| `{{PATH}}` | {{PURPOSE}} | {{NOTABLE}} |

```mermaid
flowchart TB
  subgraph repo [{{PROJECT_NAME}}]
    {{TOP_LEVEL_NODES}}
  end
```

## Stack

| Concern | Choice | Evidence |
|---------|--------|----------|
| Language | {{LANGUAGE}} | `{{PATH}}` |
| Framework / runtime | {{FRAMEWORK}} | `{{PATH}}` |
| Build tool | {{BUILD_TOOL}} | `{{PATH}}` |
| Data store | {{DATASTORE}} | `{{PATH}}` |
| Auth | {{AUTH}} | `{{PATH}}` |
| Key integrations | {{INTEGRATIONS}} | `{{PATH}}` |

## Key capabilities

| Capability | Summary | Where to look |
|------------|---------|---------------|
| {{CAPABILITY}} | {{SUMMARY}} | `{{PATH}}` |

## Further reading

- Project README: `{{README_PATH}}`
- Source entry point: `{{ENTRY_POINT}}`
- Architecture: [architecture.md](./architecture.md)
- API: [api-reference.md](./api-reference.md)
