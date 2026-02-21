# sync-agents

A single Bash script that synchronizes configuration files between [Claude Code](https://code.claude.com/docs) and [OpenAI Codex](https://openai.com/index/introducing-codex/)/Agents formats.

It keeps these pairs in sync, both project-local and global:

| Claude Code           | OpenAI Codex/Agents  |
| --------------------- | -------------------- |
| `CLAUDE.md`           | `AGENTS.md`          |
| `.claude/skills/`     | `.agents/skills/`    |
| `~/.claude/CLAUDE.md` | `~/.codex/AGENTS.md` |
| `~/.claude/skills/`   | `~/.agents/skills/`  |

By default it creates **symlinks**; with `--hard` it copies instead. The Claude-side path is always treated as the canonical source.

## Usage

```bash
./sync-agents              # Preview changes (dry-run, no arguments)
./sync-agents --local      # Sync only current directory
./sync-agents --global     # Sync only home directory
./sync-agents --skills-only # Sync only .claude/skills/ <-> .agents/skills/
./sync-agents --config-only # Sync only CLAUDE.md <-> AGENTS.md
./sync-agents --dry-run    # Explicitly preview without changes
./sync-agents --hard       # Copy instead of symlink
```

Running with **no arguments** defaults to dry-run mode so you can safely preview what would happen. Pass `--local`, `--global`, or both to actually apply changes. Use `--skills-only` or `--config-only` to sync only skills directories or only the config files (AGENTS.md/CLAUDE.md).

When applying changes, each create/link/copy operation prompts for confirmation (`[y/N]`) before proceeding.

## Sync logic

For each pair of paths the script:

1. **Already symlinked** to each other — skip (nothing to do).
2. **Both exist independently** — skip with a warning to resolve manually.
3. **Only one side exists** — symlink (or copy) the missing side from the existing one, after confirmation.
4. **Neither exists** — skip (nothing to do).

## Installation

```bash
curl -fsSL https://raw.githubusercontent.com/yilinfang/sync-agents/main/sync-agents -o sync-agents
chmod +x sync-agents
```

Or clone the repo:

```bash
git clone https://github.com/yilinfang/sync-agents.git
```
