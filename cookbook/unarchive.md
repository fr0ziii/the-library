# Unarchive an Entry

## Context
Move an entry from the `archived:` section back to the active `library:` section, restoring its visibility in `/library list`.

## Input
The user provides the name of the entry to restore.

## Steps

### 1. Sync the Library Repo
Pull the latest catalog before modifying:
```bash
cd <LIBRARY_SKILL_DIR>
git pull
```

### 2. Find the Entry
- Read `library.yaml`
- Search for the entry by name in `archived.skills`, `archived.agents`, `archived.prompts`, and `archived.rules`
- If not found in `archived:`, check if it exists in the active `library:` section and tell the user: `"<name>" is already active — no need to unarchive.`
- Note the entry's type (skill, agent, prompt, or rule)

### 3. Move the Entry
- Cut the full entry block (name, description, source, tags, requires) from `archived.<type>`
- Paste it back into `library.<type>`, inserting alphabetically by name
- If the archived subsection list is now empty (e.g., `archived.skills: []`), leave it as an empty list

### 4. Commit and Push
```bash
cd <LIBRARY_SKILL_DIR>
git add library.yaml
git commit -m "library: unarchived <type> <name>"
git push
```

### 5. Confirm
Tell the user:
```
Restored "<name>" to the active catalog.
- Install it: /library use <name>
- View catalog: /library list
```
