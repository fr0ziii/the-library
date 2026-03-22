# Archive an Entry

## Context
Move an entry from the active `library:` section to the `archived:` section. Archived entries are hidden from `/library list` by default but remain usable via `/library use`.

## Input
The user provides the name of the entry to archive.

## Steps

### 1. Sync the Library Repo
Pull the latest catalog before modifying:
```bash
cd <LIBRARY_SKILL_DIR>
git pull
```

### 2. Find the Entry
- Read `library.yaml`
- Search for the entry by name in `library.skills`, `library.agents`, `library.prompts`, and `library.rules`
- If not found, tell the user: `"<name>" not found in the active catalog. It may already be archived — check with /library list --archived.`
- Note the entry's type (skill, agent, prompt, or rule)

### 3. Move the Entry
- Cut the full entry block (name, description, source, tags, requires) from its current position in `library.<type>`
- Paste it into `archived.<type>` (create the key if it doesn't exist, or append to the existing list)
- Maintain alphabetical order within the archived section

### 4. Commit and Push
```bash
cd <LIBRARY_SKILL_DIR>
git add library.yaml
git commit -m "library: archived <type> <name>"
git push
```

### 5. Confirm
Tell the user:
```
Archived "<name>". It's now hidden from /library list.
- Still usable: /library use <name>
- Restore anytime: /library unarchive <name>
```
