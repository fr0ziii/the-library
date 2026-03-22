# List Available Skills

## Context
Show the full library catalog with install status. Supports tag filtering and archived visibility.

## Flags
- `--tag <name>` — filter results to entries whose `tags` list contains the given tag
- `--archived` — include archived entries in a separate section below active ones

## Steps

### 1. Sync the Library Repo
Pull the latest catalog before reading:
```bash
cd <LIBRARY_SKILL_DIR>
git pull
```

### 2. Read the Catalog
- Read `library.yaml`
- Parse all entries from `library.skills`, `library.agents`, `library.prompts`, and `library.rules`
- If `--archived` flag present, also parse `archived.skills`, `archived.agents`, `archived.prompts`, `archived.rules`

### 3. Apply Tag Filter
If `--tag <name>` was provided:
- Keep only entries whose `tags` list contains the given tag (case-insensitive)
- Apply to both active and archived sets if `--archived` is also present

### 4. Check Install Status
For each entry:
- Determine the type and corresponding default/global directories from `default_dirs`
- Check if a directory matching the entry name exists in the **default** directory
- Check if a directory matching the entry name exists in the **global** directory
- Search recursively for name matches
- Mark as: `installed (default)`, `installed (global)`, or `not installed`

### 5. Display Results

Format the output as a table grouped by type. Include a `Tags` column:

```
## Skills
| Name | Description | Tags | Status |
|------|-------------|------|--------|
| skill-name | skill-description | cloudflare, tools | installed (default) |
| other-skill | other-description | frontend | not installed |

## Agents
| Name | Description | Tags | Status |
|------|-------------|------|--------|

## Prompts
| Name | Description | Tags | Status |
|------|-------------|------|--------|

## Rules
| Name | Description | Tags | Status |
|------|-------------|------|--------|
```

If a section is empty (or filtered to empty), show: `No <type> in catalog.`

If `--archived` flag was present, show archived entries below in a clearly labeled section:

```
---
## Archived Skills
| Name | Description | Tags | Status |
|------|-------------|------|--------|
| old-skill | description | cloudflare | not installed |
```

### 6. Summary
At the bottom, show:
- Total active entries in catalog
- Total installed locally
- Total not installed

If there are archived entries and `--archived` was NOT passed, append:
```
(X archived entries hidden — use /library list --archived to see them)
```
