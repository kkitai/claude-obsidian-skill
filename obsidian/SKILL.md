---
name: obsidian
description: Skill for setting up an Obsidian vault linked to a development project. Use this skill when the user types "/obsidian", or requests anything like "integrate with Obsidian", "set up Obsidian", "configure vault", or "connect Obsidian to my project". Creates an Obsidian vault for the current project and sets up the .claude/ folder structure (settings.local.json, commands/, rules/) needed for Claude Code integration. Builds an environment for centrally managing tech stack info, daily task logs, meeting notes, and research.
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
  │   ├── commands/             # Custom slash commands
  │   │   ├── daily.md          # Daily task log
  │   │   ├── inbox-review.md   # Inbox triage
  │   │   ├── research.md       # Cross-vault search
  │   │   └── mtg.md            # Meeting log structuring
  │   └── rules/
  │       └── folder-structure.md  # Folder structure rules
  ├── 00_Inbox/                 # Temporary drop zone for unprocessed notes
  ├── 01_Projects/              # Per-project notes
  │   └── <project-name>/
  ├── 02_Areas/                 # Ongoing areas of interest
  ├── 03_Resources/             # Reference material & research
  ├── 04_Archive/               # Archive
  └── README.md

Proceed? (yes/no):
```

- If the user answers **no** → abort setup
- If the user answers **yes** → continue to Step 3
- If the user wants a custom vault path, accept it, update the display above, and ask for confirmation again

### Step 3: Create Vault Directory Structure

Once the user approves, create the following:

```bash
mkdir -p <vault-path>/.claude/commands
mkdir -p <vault-path>/.claude/rules
mkdir -p "<vault-path>/00_Inbox"
mkdir -p "<vault-path>/01_Projects/<project-name>"
mkdir -p "<vault-path>/02_Areas"
mkdir -p "<vault-path>/03_Resources"
mkdir -p "<vault-path>/04_Archive"
```

### Step 4: Create Configuration Files

Create the files listed below. Refer to `references/vault_structure.md` for the exact template content of each file. Replace `<project-name>` and `<project-path>` with the actual values.

**Files to create:**
1. `<vault-path>/.claude/settings.local.json` — tech stack & personal settings
2. `<vault-path>/.claude/commands/daily.md` — /daily command
3. `<vault-path>/.claude/commands/inbox-review.md` — /inbox-review command
4. `<vault-path>/.claude/commands/research.md` — /research command
5. `<vault-path>/.claude/commands/mtg.md` — /mtg command
6. `<vault-path>/.claude/rules/folder-structure.md` — folder structure rules
7. `<vault-path>/README.md` — vault description

### Step 5: Display Completion Message

```
✓ Obsidian Vault setup complete!

  Vault path: <vault-path>

Next steps:
1. Open this folder as a Vault in Obsidian
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
