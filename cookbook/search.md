# Search the Library

## Context
Find entries in the catalog by keyword, name, or tag.

## Input
The user provides a keyword, description fragment, or `--tag <name>` flag.

## Flags
- `--tag <name>` — filter exclusively by tag match instead of full-text search

## Steps

### 1. Sync the Library Repo
Pull the latest catalog before reading:
```bash
cd <LIBRARY_SKILL_DIR>
git pull
```

### 2. Read the Catalog
- Read `library.yaml`
- Parse all entries from **both** `library` and `archived` sections (all four types each)

### 3. Search
If `--tag <name>` flag provided:
- Match entries whose `tags` list contains the given tag (case-insensitive exact match)

Otherwise (keyword search):
- Match the keyword (case-insensitive) against:
  - Entry `name`
  - Entry `description`
  - Entry `tags` list (any tag contains the keyword as a substring)
- A match is any entry where the keyword appears in any of these fields
- Collect all matches across all types and both sections

### 4. Display Results

If matches found, format as:

```
## Search Results for "<keyword>"

| Type | Name | Tags | Description | Source | Status |
|------|------|------|-------------|--------|--------|
| skill | matching-skill | cloudflare | description... | source... | active |
| skill | old-skill | frontend | description... | source... | archived |
| agent | matching-agent | tools | description... | source... | active |
```

Mark entries from the `archived:` section with `archived` in the Status column.

If no matches:
```
No results found for "<keyword>".

Tip: Try broader keywords or run `/library list` to see the full catalog.
```

### 5. Suggest Next Step
If matches were found, suggest: `Run /library use <name> to install one of these.`
If archived results were returned, add: `Archived entries can be restored with /library unarchive <name>.`
