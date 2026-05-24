---
name: obsidian
description: Skill for setting up an Obsidian vault linked to a development project. Use this skill when the user types "/obsidian", or requests anything like "integrate with Obsidian", "set up Obsidian", "configure vault", or "connect Obsidian to my project". Creates an Obsidian vault for the current project and sets up the .claude/ folder structure (settings.local.json, commands/) needed for Claude Code integration. Builds an environment for centrally managing tech stack info, daily task logs, meeting notes, and research.
---

# Obsidian Integration Setup

This skill sets up a linked environment between the Obsidian vault and Claude Code for the development project in the current directory.

## Setup Flow

### Step 1: Collect Project Information

First, confirm and collect the following information:

**Determine the project name**
```bash
# Check whether the current directory is a git repository
git rev-parse --is-inside-work-tree 2>/dev/null
```

- If it is a git repository → derive the project name with `basename $(git rev-parse --show-toplevel)`
- If it is not a git repository → prompt the user for a project name:
  ```
  The current directory is not a git repository.
  Please enter a project name (e.g. my-project):
  ```

**Confirm paths**
- Project path: absolute path of the current directory
- Obsidian vault path (default): `~/Documents/Obsidian/<project-name>`
- If the user supplies a vault path as an argument, use that instead

### Step 2: Present Configuration and Ask for Confirmation

Display the following to the user and ask whether to proceed:

```
The following Obsidian integration will be set up:

  Project path  : <project-path>
  Obsidian Vault: <vault-path>

  Vault structure to be created:
  <vault-path>/
  ├── .claude/
  │   ├── settings.local.json   # Tech stack & personal settings
  │   └── commands/             # Custom slash commands
  │       ├── daily.md          # Daily task log
  │       ├── inbox-review.md   # Inbox triage
  │       ├── research.md       # Cross-vault search
  │       └── mtg.md            # Meeting log structuring
  ├── 00_Inbox/                 # Temporary drop zone for unprocessed notes
  ├── 01_Plan/                  # Plans, decisions, daily logs, meeting notes
  │   ├── daily/
  │   ├── meetings/
  │   └── docs/
  ├── 02_Areas/                 # Ongoing areas of interest
  ├── 03_Resources/             # Reference material & research
  ├── 04_Archive/               # Archive
  ├── Attachments/              # Images and media files
  ├── Templates/                # Obsidian note templates
  └── README.md

  Also created in the project directory:
  <project-path>/
  └── CLAUDE.md                 # Rules for referencing the Obsidian vault

Proceed? (yes/no):
```

- If the user answers **no** → abort setup
- If the user answers **yes** → continue to Step 3
- If the user wants a custom vault path, accept it, update the display above, and ask for confirmation again

### Step 3: Execute a Single Setup Script

After the user confirms in Step 2, read the file content templates from `references/vault_structure.md`, then generate and execute a single bash script like the one below. Replace `<vault-path>`, `<project-path>`, `<project-name>`, and `<today>` with the actual values before running.

```bash
#!/usr/bin/env bash
set -e

VAULT="<vault-path>"
PROJECT="<project-path>"
PROJECT_NAME="<project-name>"
TODAY="<today>"   # YYYY-MM-DD

# --- Directories ---
mkdir -p "$VAULT/.claude/commands"
mkdir -p "$VAULT/00_Inbox"
mkdir -p "$VAULT/01_Plan/daily"
mkdir -p "$VAULT/01_Plan/meetings"
mkdir -p "$VAULT/01_Plan/docs"
mkdir -p "$VAULT/02_Areas"
mkdir -p "$VAULT/03_Resources"
mkdir -p "$VAULT/04_Archive"
mkdir -p "$VAULT/Attachments"
mkdir -p "$VAULT/Templates"

# --- .claude/settings.local.json ---
cat > "$VAULT/.claude/settings.local.json" << 'HEREDOC'
<exact content from references/vault_structure.md — settings.local.json section>
HEREDOC

# --- .claude/commands/daily.md ---
cat > "$VAULT/.claude/commands/daily.md" << 'HEREDOC'
<exact content from references/vault_structure.md — daily.md section>
HEREDOC

# --- .claude/commands/inbox-review.md ---
cat > "$VAULT/.claude/commands/inbox-review.md" << 'HEREDOC'
<exact content from references/vault_structure.md — inbox-review.md section>
HEREDOC

# --- .claude/commands/research.md ---
cat > "$VAULT/.claude/commands/research.md" << 'HEREDOC'
<exact content from references/vault_structure.md — research.md section>
HEREDOC

# --- .claude/commands/mtg.md ---
cat > "$VAULT/.claude/commands/mtg.md" << 'HEREDOC'
<exact content from references/vault_structure.md — mtg.md section>
HEREDOC

# --- README.md ---
cat > "$VAULT/README.md" << HEREDOC
<exact content from references/vault_structure.md — README.md section,
 with $PROJECT_NAME and $PROJECT substituted>
HEREDOC

# --- CLAUDE.md in the project directory ---
CLAUDE_MD="$PROJECT/CLAUDE.md"
OBSIDIAN_SECTION=$(cat << HEREDOC
<exact content from references/vault_structure.md — CLAUDE.md section,
 with $PROJECT_NAME, $PROJECT, $VAULT substituted>
HEREDOC
)

if [ -f "$CLAUDE_MD" ]; then
  printf '\n%s\n' "$OBSIDIAN_SECTION" >> "$CLAUDE_MD"
else
  printf '%s\n' "$OBSIDIAN_SECTION" > "$CLAUDE_MD"
fi

echo "done"
```

Fill in all `<exact content …>` placeholders with the real text from `references/vault_structure.md` before executing. The script must be complete and self-contained so the entire setup runs in one Bash tool call.

### Step 4: Display Completion Message

```
✓ Obsidian Vault setup complete!

  Vault path  : <vault-path>
  CLAUDE.md   : <project-path>/CLAUDE.md (created/updated)

Next steps:
1. Open <vault-path> as a Vault in Obsidian
2. Fill in your tech stack and personal details in .claude/settings.local.json
3. Launch claude in the vault folder and try the commands:
   - /daily        → organize today's tasks and log
   - /inbox-review → get triage suggestions for unprocessed notes
   - /research     → cross-search notes in the vault
   - /mtg          → structure a meeting log
```

## Error Handling

- If the vault path already exists: ask the user whether to overwrite
- If directory creation fails due to permissions: show an error and suggest an alternative path
- If the project name contains special characters: convert to a filesystem-safe name (e.g. spaces → hyphens)
