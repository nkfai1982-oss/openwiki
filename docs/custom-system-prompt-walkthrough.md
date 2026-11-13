# Custom System Prompt Implementation & `dhl-openwiki` Packaging

This document outlines the custom system prompt override feature and packaging for **`dhl-openwiki`**.

---

## Overview

OpenWiki now supports overriding the repository system prompt to define your own documentation folder structure, guide sections, and levels of granularity via environment variables in **both Code Mode and Personal Mode**.

Sample customized prompt specification: [`custom-system-prompt.md`](../custom-system-prompt.md).

---

## Configuration (DHL Customization)

Two environment variables are supported:

| Environment Variable | Description |
| :--- | :--- |
| `OPENWIKI_CUSTOM_SYSTEM_PROMPT` | Inline raw string containing your custom prompt specification. |
| `OPENWIKI_CUSTOM_SYSTEM_PROMPT_FILE` | Relative or absolute path to a Markdown prompt file (e.g. `custom-system-prompt.md`). |

### Precedence
1. `OPENWIKI_CUSTOM_SYSTEM_PROMPT` (highest priority)
2. `OPENWIKI_CUSTOM_SYSTEM_PROMPT_FILE`
3. `openwiki/INSTRUCTIONS.md` (`wikiGoal` fallback)
4. Built-in default documentation architecture

---

## How It Works

### Code Mode (`--init` / `--update`)
- **Planning Agent (`createRepositoryPlannerPrompt`)**:
  - Replaces default hardcoded directories (`/openwiki/architecture/`, etc.) with the guide sections, objectives, and granularity requirements defined in your custom prompt.
  - Instructs the planner to map each custom section into planned pages under `/openwiki/` and copy each section's objective and granularity rules into that page's `instructions` array.
  - Retains the essential invariant that `/openwiki/quickstart.md` must be included for `init`.
- **Page Worker Agents (`createRepositoryPagePrompt`)**:
  - Injects `<custom_documentation_guidelines>` containing the custom prompt into each worker's prompt so that each page strictly adheres to the requested level of granularity.

### Personal Mode (`personal --init` / `personal --update`)
- The custom prompt is loaded into the run context `{WIKI_GOAL}` and appended to `createSystemPrompt` (`outputMode: "local-wiki"`), ensuring personal brain runs follow the custom guidelines.

---

## Packaging as `dhl-openwiki`

The repository is configured as `dhl-openwiki` in `package.json` to allow independent local installation and internal distribution without conflicting with upstream `openwiki`.

### 1. Build and Pack Tarball
```powershell
npm run build
npm pack
```
This generates:
```text
dhl-openwiki-0.9.0.tgz
```

### 2. Local Installation
```powershell
npm install -g ./dhl-openwiki-0.9.0.tgz
```

Verify the binary is available (both `dhl-openwiki` and `openwiki` commands are available):
```powershell
dhl-openwiki --help
# Or using the openwiki alias:
openwiki --help
```

### 3. Usage with Custom Prompt
```powershell
# Using a custom prompt file
$env:OPENWIKI_CUSTOM_SYSTEM_PROMPT_FILE = "custom-system-prompt.md"
dhl-openwiki --init

# Or inline prompt string
$env:OPENWIKI_CUSTOM_SYSTEM_PROMPT = "Your custom prompt..."
dhl-openwiki --init
```

### 4. Publishing to Internal Artifactory
```powershell
npm publish --registry https://<your-artifactory-host>/artifactory/api/npm/<repo-name>/
```
