# Development guide

## Prerequisites

| Tool | Version / notes | Source |
|------|-----------------|--------|
| {{TOOL}} | {{VERSION}} | `{{FILE}}` |

## Getting started

### Clone and install

```bash
{{CLONE_AND_INSTALL_STEPS}}
```

### Environment

Copy or create environment configuration as required by the project:

```bash
{{ENV_SETUP_COMMANDS}}
```

| Variable | Required | Description |
|----------|----------|-------------|
| `{{VAR}}` | {{YES_NO}} | {{DESCRIPTION}} |

## Build

```bash
{{BUILD_COMMAND}}
```

## Run locally

```bash
{{RUN_COMMAND}}
```

{{RUN_NOTES}}

## Test

```bash
{{TEST_COMMAND}}
```

{{TEST_NOTES}}

## Lint and format

```bash
{{LINT_COMMANDS}}
```

## Project layout

| Path | Description |
|------|-------------|
| `{{PATH}}` | {{DESCRIPTION}} |

## CI/CD

<!-- Summarize from .github/workflows, .gitlab-ci.yml, etc. if present -->

{{CI_SUMMARY}}

## Troubleshooting

### {{ISSUE}}

**Symptom:** {{SYMPTOM}}

**Fix:** {{FIX}}

## Contributing

<!-- From CONTRIBUTING.md or inferred from PR templates -->

{{CONTRIBUTING_NOTES}}
