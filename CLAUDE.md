## Overview

`sync-agents` is a single Bash script that synchronizes configuration files between Claude Code and OpenAI Codex/Agents formats:

- `CLAUDE.md` <-> `AGENTS.md` (project-local and global `~/.claude/CLAUDE.md` <-> `~/.codex/AGENTS.md`)
- `.claude/skills/` <-> `.agents/skills/` (project-local and global)

By default it creates symlinks; with `--hard` it copies instead.

## Running

```bash
./sync-agents               # Preview changes (dry-run, default scope is local)
./sync-agents --local       # Deprecated: same as default local scope
./sync-agents --global      # Sync only home directory
./sync-agents --all         # Sync both current directory and home directory
./sync-agents --local --global # Sync both local and global
./sync-agents --skills-only # Sync only .claude/skills/ <-> .agents/skills/
./sync-agents --config-only # Sync only CLAUDE.md <-> AGENTS.md
./sync-agents --dry-run     # Explicitly preview without changes
./sync-agents --hard        # Copy instead of symlink
```

Running with no arguments defaults to dry-run. Scope defaults to local. Use `--global` for global only, `--all` for both, or `--local --global` for both. `--local` is kept for compatibility but deprecated. Each create/link/copy operation prompts for user confirmation.

## Architecture

The script (`sync-agents`) uses a single core function `sync_item` that handles the bidirectional sync logic for a pair of paths:

1. If already symlinked to each other, skip.
2. If both exist independently, skip with a manual-resolution warning.
3. If only one exists, link (or copy) the missing one from the existing one (with confirmation).
4. If neither exists, skip.
