# claude-obsidian-template

A skill repository for integrating Claude Code with Obsidian.

## Skills

### `/obsidian`

Sets up an Obsidian vault linked to a development project.

**What it does:**
- Creates an Obsidian vault (default: `~/Documents/Obsidian/<project-name>`)
- Sets up the `.claude/` folder structure required for Claude Code integration
- Installs custom slash commands (`/daily`, `/inbox-review`, `/research`, `/mtg`)
- Creates a PARA-method folder structure inside the vault

**Usage:**
```
/obsidian
```

## Installation

```bash
# Add this repo as a marketplace (one-time setup)
claude plugin marketplace add kkitai/claude-obsidian-template

# Install the plugin
claude plugin install obsidian
```

## Reference

- [Claude Code + Obsidian Integration Guide](https://note.com/antrefreepreneur/n/nabdcbc1aa26b)
