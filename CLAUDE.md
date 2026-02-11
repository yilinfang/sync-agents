## Overview

`sync-agents` is a single Bash script that synchronizes configuration files between Claude Code and OpenAI Codex/Agents formats:

- `CLAUDE.md` <-> `AGENTS.md` (project-local and global `~/.claude/CLAUDE.md` <-> `~/.codex/AGENTS.md`)
- `.claude/skills/` <-> `.agents/skills/` (project-local and global)

By default it creates symlinks; with `--hard` it copies instead.

## Running

```bash
./sync-agents              # Preview changes (dry-run, no arguments)
./sync-agents --local      # Sync only current directory
./sync-agents --global     # Sync only home directory
./sync-agents --dry-run    # Explicitly preview without changes
./sync-agents --hard       # Copy instead of symlink
```

Running with no arguments defaults to dry-run. Pass `--local`, `--global`, or both to apply changes. Each create/link/copy operation prompts for user confirmation.

## Architecture

The script (`sync-agents`) uses a single core function `sync_item` that handles the bidirectional sync logic for a pair of paths:

1. If already symlinked to each other, skip.
2. If both exist independently, skip with a manual-resolution warning.
3. If only one exists, link (or copy) the missing one from the existing one (with confirmation).
4. If neither exists, skip.
