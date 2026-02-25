## Overview

`sync-agents` is a single Bash script that synchronizes configuration files between Claude Code, OpenAI Codex/Agents, and Google Gemini/Antigravity formats:

- Config files: `CLAUDE.md` / `AGENTS.md` (project-local), `~/.claude/CLAUDE.md` / `~/.codex/AGENTS.md` / `~/.gemini/GEMINI.md` (global)
- Skills directories: `.claude/skills/` / `.agents/skills/` / `.gemini/antigravity/skills/` (project-local and global)

By default it creates symlinks; with `--hard` it copies instead.

## Running

```bash
./sync-agents               # Preview changes (dry-run, default scope is local)
./sync-agents --local       # Deprecated: same as default local scope
./sync-agents --global      # Sync only home directory
./sync-agents --all         # Sync both current directory and home directory
./sync-agents --local --global # Sync both local and global
./sync-agents --skills-only # Sync only skills directories
./sync-agents --config-only # Sync only config files
./sync-agents --dry-run     # Explicitly preview without changes
./sync-agents --hard        # Copy instead of symlink
```

Running with no arguments defaults to dry-run. Scope defaults to local. Use `--global` for global only, `--all` for both, or `--local --global` for both. `--local` is kept for compatibility but deprecated. Each create/link/copy operation prompts for user confirmation.

## Architecture

The script (`sync-agents`) uses a single core function `sync_group` that handles the sync logic for a group of paths:

1. If none exist, skip.
2. Among existing items: if already symlinked to each other, report ok. If any exist independently (conflict), skip the entire group with a manual-resolution warning.
3. If no conflicts, link (or copy) any missing items from the first existing one (with confirmation).
