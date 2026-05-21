# Vault Structure Templates

This file defines the exact content for each configuration file.
Replace `<project-name>` and `<project-path>` with the actual values.

---

## .claude/settings.local.json

```json
{
  "env": {},
  "notes": {
    "project": "<project-name>",
    "project_path": "<project-path>",
    "tech_stack": {
      "language": "TODO: enter your language (e.g. TypeScript, Python)",
      "framework": "TODO: enter your framework (e.g. Next.js, FastAPI)",
      "database": "TODO: enter your database (e.g. PostgreSQL, SQLite)",
      "infra": "TODO: enter your infra (e.g. Vercel, AWS)"
    },
    "preferences": {
      "language": "en",
      "date_format": "YYYY-MM-DD",
      "note_style": "zettelkasten"
    }
  }
}
```

---

## .claude/commands/daily.md

```markdown
Run a daily review for $ARGUMENTS (or today's date if not specified).

Steps:
1. Check 00_Inbox/ for any unprocessed notes
2. Review in-progress tasks under 01_Projects/<project-name>/
3. Create or update the daily note in the following format:

File path: 01_Projects/<project-name>/daily/YYYY-MM-DD.md

## Daily Log: YYYY-MM-DD

### Today's tasks
- [ ] 

### Completed


### Carried over to tomorrow
- [ ] 

### Notes & insights

```

---

## .claude/commands/inbox-review.md

```markdown
Triage and organize unprocessed notes in 00_Inbox/.

Steps:
1. List all files in 00_Inbox/
2. Read the content of each file
3. Suggest a destination for each file based on these criteria:
   - Project-related → 01_Projects/<project-name>/
   - Ongoing theme → 02_Areas/
   - Reference / research → 03_Resources/
   - Old or no longer needed → 04_Archive/

4. Present suggestions as a table:

| File | Current location | Suggested destination | Reason |
|------|-----------------|----------------------|--------|

5. Move files only after the user confirms
```

---

## .claude/commands/research.md

```markdown
Cross-search this vault for "$ARGUMENTS" and summarize the related notes.

Steps:
1. Search all .md files in the vault for keywords related to "$ARGUMENTS"
2. List the notes in order of relevance
3. Quote the relevant passages from each note
4. Illustrate connections between related notes (as text)
5. Suggest unexplored angles or missing links

Output format:
## Research summary: "$ARGUMENTS"

### Related notes
1. [Note title](path) — summary of relevant content

### Key insights


### Unexplored angles
```

---

## .claude/commands/mtg.md

```markdown
Create a structured log from the following meeting notes.

Load $ARGUMENTS (or the most recent file in 00_Inbox/) and organize it in this format:

Save to: 01_Projects/<project-name>/meetings/YYYY-MM-DD_<meeting-name>.md

## Meeting log: <meeting-name>

- Date: YYYY-MM-DD HH:MM
- Attendees: 
- Purpose: 

### Agenda
1. 

### Minutes


### Decisions
- [ ] 

### Action items
| Task | Owner | Due date |
|------|-------|----------|

### Follow-up before next meeting
```

---

## .claude/rules/folder-structure.md

```markdown
# Vault Folder Structure Rules

This vault is managed with the following folder structure.
Always follow these rules when creating or moving files.

## Folder purposes

### 00_Inbox/
- Purpose: temporary drop zone for unprocessed notes
- Rule: notes here are temporary — triage regularly with /inbox-review
- File names: free-form (date prefix recommended: YYYYMMDD_title.md)

### 01_Projects/
- Purpose: notes for active projects
- Structure: organize under 01_Projects/<project-name>/ with subfolders
- Subfolder examples: daily/, meetings/, tasks/, docs/

### 02_Areas/
- Purpose: ongoing areas of interest that are continuously maintained
- Examples: tech learning, health, career, reading log
- Rule: for ongoing themes, not time-boxed projects

### 03_Resources/
- Purpose: reference material, research, and summaries of external information
- Examples: tech documentation notes, article summaries, book notes

### 04_Archive/
- Purpose: storage for completed or no-longer-needed notes
- Rule: move here before deleting

## File naming conventions
- Spaces → hyphens (-)
- Dates in YYYY-MM-DD format
- Use specific, descriptive titles that make the content obvious at a glance

## Linking
- Use [[Note title]] to create internal links
- Link related notes liberally
- Use #tag-name format for tags
```

---

## README.md

```markdown
# <project-name> Obsidian Vault

This vault was created for knowledge management and Claude Code integration for the "<project-name>" project.

## Project info
- Project path: <project-path>
- Created: YYYY-MM-DD

## Folder structure
- `00_Inbox/` — temporary drop zone for unprocessed notes
- `01_Projects/` — per-project notes
- `02_Areas/` — ongoing areas of interest
- `03_Resources/` — reference material & research
- `04_Archive/` — archive

## Claude Code commands

Launch `claude` in this vault folder to use the following commands:

| Command | Description |
|---------|-------------|
| `/daily` | Organize today's tasks and log |
| `/inbox-review` | Suggest triage for unprocessed notes |
| `/research <keyword>` | Cross-search notes in the vault |
| `/mtg` | Structure a meeting log |

## Configuration
Edit `.claude/settings.local.json` to add your personal tech stack and preferences.
```
