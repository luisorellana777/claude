# API reference

{{API_SURFACE_SUMMARY}}

<!-- Choose the sections that apply: HTTP API, CLI, library exports -->

## HTTP API

<!-- Skip this section if no HTTP server is present -->

Base URL: `{{BASE_URL}}` (local / default)

| Method | Path | Description | Handler / source |
|--------|------|-------------|------------------|
| {{METHOD}} | `{{PATH}}` | {{DESCRIPTION}} | `{{SOURCE_FILE}}` |

### {{ENDPOINT_NAME}}

- **Method / path:** `{{METHOD}} {{PATH}}`
- **Description:** {{DESCRIPTION}}
- **Source:** `{{SOURCE_FILE}}`

**Request**

<!-- Query, path, body parameters -->

| Parameter | In | Type | Required | Description |
|-----------|-----|------|----------|-------------|
| {{NAME}} | {{in}} | {{TYPE}} | {{REQ}} | {{DESC}} |

**Response**

{{RESPONSE_DESCRIPTION}}

**Example**

```{{LANGUAGE}}
{{EXAMPLE}}
```

---

## CLI

<!-- Skip if no CLI -->

Invocation: `{{CLI_BIN}}`

| Command | Description | Source |
|---------|-------------|--------|
| `{{COMMAND}}` | {{DESCRIPTION}} | `{{SOURCE_FILE}}` |

### `{{COMMAND}}`

{{COMMAND_DETAILS}}

---

## Library / module exports

<!-- For libraries: public classes, functions, types -->

| Export | Kind | Description | Source |
|--------|------|-------------|--------|
| `{{SYMBOL}}` | {{KIND}} | {{DESCRIPTION}} | `{{SOURCE_FILE}}` |

### `{{SYMBOL}}`

{{SYMBOL_DETAILS}}

---

## Authentication

<!-- If applicable; otherwise state "No authentication layer found" -->

{{AUTH_NOTES}}

## Errors

<!-- Common error shapes or status codes if documented in code -->

{{ERROR_NOTES}}
