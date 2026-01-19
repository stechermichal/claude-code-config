# Claude Code Config

My Claude Code settings, custom commands, and agents. This is my actual `~/.claude/` directory.

## Commands

- **`/worktree`** - Create a git worktree from a GitHub issue. Fetches issue details, generates branch name, confirms with you, then creates the worktree.

- **`/cleanup-worktrees`** - Interactive worktree cleanup. Analyzes all worktrees (merge status, activity, size), shows detailed summary for each, lets you decide what to delete.

- **`/review-staged`** - Pre-commit review using `@staged-changes-reviewer`. Checks commit coherence, tests, docs, code quality, codebase consistency.

- **`/review-unstaged`** - Review work-in-progress using `@unstaged-diff-reviewer`. Iterative review of changes since last staged checkpoint.

- **`/typecheck-fast`** - Fast TypeScript check on modified files only. Runs `tsc --noEmit` just on what you changed, not the whole project. Useful when Claude edits files and doesn't realize it introduced type errors.

- **`/catchup-context`** - Load full project context. Reads CLAUDE.md, package.json, database schema, recent migrations, current branch, commits, and diffs. Summarizes where you are and what's next.

- **`/translation-assist`** - Extract translation keys, find empty English translations, generate 3 suggestions for each, let you pick or write your own.

## Agents

- **`@database-schema-explorer`** - PostgreSQL schema inspector. Use when you need table structure, relationships, triggers, constraints. Uses `psql` commands.

- **`@staged-changes-reviewer`** - Pre-commit validator. Reviews staged changes for quality, completeness, and consistency before commit.

- **`@test-writer`** - Test generator. Two-phase approach: researches existing tests first, then writes new ones following your patterns.

- **`@unstaged-diff-reviewer`** - Iterative code reviewer. Reviews delta between staged checkpoint and current work-in-progress.

## Hooks

- **Prettier auto-format** - Runs after Edit/Write on `.ts`/`.tsx`/`.js` files

Configured in `settings.json`.

### Terminal Notifications

Set up terminal notifications so you get macOS alerts when Claude needs permission or is waiting for input. The setup depends on your terminal - see https://docs.anthropic.com/en/docs/claude-code/hooks for details.

## Settings

`settings.json` includes:
- 100+ pre-approved permissions for common dev tasks (git, yarn, npm, node, gh, file ops)
- Git-aware status line showing model, branch, and uncommitted changes
- Hooks configuration
- Enabled MCP servers (context7, puppeteer, sequential-thinking, mcp-deepwiki, chrome-devtools)

## Use Git Worktrees

If you aren't using git worktrees with Claude Code, you're doing it wrong. Worktrees let you:
- Work on multiple branches simultaneously without stashing
- Have Claude work on a feature while you review another branch
- Keep your main repo clean while experimenting

The `/worktree` and `/cleanup-worktrees` commands make this workflow seamless.

## Mobile Setup

To use Claude Code from my phone, I use Amphetamine (set to keep MacBook awake while Kitty terminal is running and lid isn't closed), Zellij for persistent terminal sessions, and Termius on my phone to SSH in and reattach.

## Installation

Clone to your home directory:

```bash
git clone https://github.com/stechermichal/claude-code-config.git ~/.claude
```

Or if you already have a `~/.claude` directory, cherry-pick what you want:

```bash
git clone https://github.com/stechermichal/claude-code-config.git /tmp/claude-config
cp /tmp/claude-config/commands/* ~/.claude/commands/
cp /tmp/claude-config/agents/* ~/.claude/agents/
# Merge settings manually
```

## Notes

- Commands and agents reference common conventions (Prisma, monorepo structure, Jest/Vitest) - adjust to match your stack
- The status line in settings.json shows git branch and status - works on any repo
- MCP servers need to be installed separately - this just enables them
