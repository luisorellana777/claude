# Development guide

Deep, practical guide to set up, run, test, and change this project. Commands and versions must match project files.

## Prerequisites

| Tool | Version / notes | Source |
|------|-----------------|--------|
| {{TOOL}} | {{VERSION}} | `{{FILE}}` |

## Getting started

### Clone and install

```bash
{{CLONE_AND_INSTALL_STEPS}}
```

### Environment and profiles

Explain how configuration is loaded (e.g. Spring profiles, `.env`, `application-*.yml`).

```bash
{{ENV_SETUP_COMMANDS}}
```

| Variable / key | Required | Description | Source |
|----------------|----------|-------------|--------|
| `{{VAR}}` | {{YES_NO}} | {{DESCRIPTION}} | `{{PATH}}` |

| Profile / env | Purpose | How to activate | Source |
|---------------|---------|-----------------|--------|
| {{PROFILE}} | {{PURPOSE}} | {{HOW}} | `{{PATH}}` |

## Build

```bash
{{BUILD_COMMAND}}
```

**Artifacts produced:** {{ARTIFACTS}}

**Build notes:** {{BUILD_NOTES}}

## Run locally

```bash
{{RUN_COMMAND}}
```

**Expected startup behavior:** {{STARTUP_NOTES}}

**Local dependencies** (DB, emulators, Docker Compose services):

| Dependency | How to start | Source |
|------------|--------------|--------|
| {{DEP}} | {{COMMAND_OR_COMPOSE}} | `{{PATH}}` |

## Test

```bash
{{TEST_COMMAND}}
```

| Test type | Location | How to run | Notes |
|-----------|----------|------------|-------|
| {{TYPE}} | `{{PATH}}` | `{{CMD}}` | {{NOTES}} |

## Lint, format, and static analysis

```bash
{{LINT_COMMANDS}}
```

## Project layout (detailed)

| Path | Description | Notable contents |
|------|-------------|------------------|
| `{{PATH}}` | {{DESCRIPTION}} | {{NOTABLE}} |

```mermaid
flowchart TB
  subgraph repo [Repository]
    {{LAYOUT_NODES}}
  end
```

## Local architecture for developers

Short orientation for someone opening the repo for the first time: where to change API, business logic, persistence, and integrations.

| Concern | Start here | Evidence |
|---------|------------|----------|
| HTTP / API | `{{PATH}}` | `{{PATH}}` |
| Business logic | `{{PATH}}` | `{{PATH}}` |
| Persistence | `{{PATH}}` | `{{PATH}}` |
| External AI / clients | `{{PATH}}` | `{{PATH}}` |
| Security | `{{PATH}}` | `{{PATH}}` |

## Docker / Compose / Kubernetes

<!-- Skip sections with no evidence -->

### Docker

{{DOCKER_NOTES}}

### Compose

{{COMPOSE_NOTES}}

### Kubernetes / deployment manifests

{{K8S_NOTES}}

## CI/CD

{{CI_SUMMARY}}

| Pipeline / job | Trigger | What it does | Source |
|----------------|---------|--------------|--------|
| {{JOB}} | {{TRIGGER}} | {{DESC}} | `{{PATH}}` |

## Debugging tips

| Scenario | What to check | Where |
|----------|---------------|-------|
| {{SCENARIO}} | {{CHECK}} | `{{PATH}}` |

## Troubleshooting

### {{ISSUE}}

**Symptom:** {{SYMPTOM}}

**Likely cause:** {{CAUSE}}

**Fix:** {{FIX}}

**Evidence:** `{{PATH}}`

## Contributing

{{CONTRIBUTING_NOTES}}

## Gaps

- {{GAP_OR_TBD}}
