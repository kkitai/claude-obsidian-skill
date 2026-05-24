# Vault Structure Templates

This file defines the exact content for each configuration file.
Replace `<project-name>` and `<project-path>` with the actual values.

---

## .claude/settings.local.json

```json
{
  "env": {
    "VAULT_ROOT": "~/Documents/Obsidian/<project-name>",
    "DAILY_FOLDER": "01_Plan/daily",
    "INBOX_FOLDER": "00_Inbox",
    "PROJECT_FOLDER": "01_Plan",
    "TEMPLATE_FOLDER": "Templates",
    "ARCHIVE_FOLDER": "04_Archive",
    "TIMEZONE": "Asia/Tokyo",
    "LOCALE": "en",
    "DATE_FORMAT": "YYYY-MM-DD",
    "DATETIME_FORMAT": "YYYY-MM-DD HH:mm"
  }
}
```

---

## .claude/commands/daily.md

```markdown
Run a daily review for $ARGUMENTS (or today's date if not specified).

Steps:
1. Check 00_Inbox/ for any unprocessed notes
2. Review in-progress tasks under 01_Plan/
3. Create or update the daily note in the following format:

File path: 01_Plan/daily/YYYY-MM-DD.md

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
   - Plans, decisions, logs → 01_Plan/docs/
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

Save to: 01_Plan/meetings/YYYY-MM-DD_<meeting-name>.md

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

## README.md

```markdown
# <project-name> Obsidian Vault

This vault was created for knowledge management and Claude Code integration for the "<project-name>" project.

## Project info
- Project path: <project-path>
- Created: YYYY-MM-DD

## Folder structure

### 00_Inbox/
Temporary drop zone for unprocessed notes. Triage regularly with `/inbox-review`.
- File names: free-form (date prefix recommended: `YYYYMMDD_title.md`)

### 01_Plan/
Plans, decisions, daily logs, and meeting notes for this project.
- `daily/` — daily logs (managed by `/daily`; do not write here directly)
- `meetings/` — meeting logs (managed by `/mtg`)
- `docs/` — decisions, architecture docs, project notes

### 02_Areas/
Ongoing areas of interest that are continuously maintained (e.g. tech learning, career, reading log).
For ongoing themes, not time-boxed projects.

### 03_Resources/
Reference material, research, and summaries of external information (e.g. tech docs, article summaries, book notes).

### 04_Archive/
Storage for completed or no-longer-needed notes. Move here before deleting.

### Attachments/
Images, PDFs, and other media files linked in notes. Store media files here when inserting into a note.

### Templates/
Obsidian note templates for consistent note structure.
Configure in Obsidian Settings → Templates → Template folder location.

## File naming conventions
- Spaces → hyphens (`-`)
- Dates in `YYYY-MM-DD` format
- Use specific, descriptive titles that make the content obvious at a glance

## Linking
- Use `[[Note title]]` for internal links
- Link related notes liberally
- Use `#tag-name` for tags

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

---

## CLAUDE.md (placed in the development project directory)

This file is created at `<project-path>/CLAUDE.md`. It gives Claude persistent awareness of the linked Obsidian vault so it can reference and update notes automatically during development work.

```markdown
## Obsidian Vault Integration

This project is linked to an Obsidian vault at: <vault-path>

For the vault's folder structure, subfolders, file naming conventions, and linking rules, read `<vault-path>/README.md`.

### When to reference the vault
- Before starting work on a feature or investigation, check `<vault-path>/01_Plan/` for relevant notes and prior decisions.
- When the user asks about something that may have been researched before, search the vault first.

### When to write to the vault

#### During a work session
Capture raw, unprocessed notes to `<vault-path>/00_Inbox/` at any point during work:
- Quick ideas or observations that come up mid-task
- Links, code snippets, or references worth keeping
- Raw meeting notes before they are structured

File name format: `YYYYMMDD_HHMM_<short-description>.md`

#### At the end of a work session
- If anything noteworthy happened (decisions, blockers, insights), suggest dropping a brief session note in `<vault-path>/00_Inbox/`.
- Remind the user to run `/daily` in the vault folder to process Inbox items into the daily log.

#### After a decision is made
Offer to save the decision and its rationale as a note in `<vault-path>/01_Plan/docs/`.

#### After a meeting or discussion
Suggest running `/mtg` in the vault folder to create a structured meeting log.

#### When new reference material is found
Suggest saving a summary to `<vault-path>/03_Resources/`.

### Vault commands (run claude in the vault folder)
| Command | Description |
|---------|-------------|
| `/daily` | Organize today's tasks and log |
| `/inbox-review` | Suggest triage for unprocessed notes |
| `/research <keyword>` | Cross-search notes in the vault |
| `/mtg` | Structure a meeting log |
```
