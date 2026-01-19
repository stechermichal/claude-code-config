## Instructions

You will help the user interactively clean up git worktrees for the current project.

### Phase 1: Scan & Analyze All Worktrees

First, get the list of all worktrees:
```bash
git worktree list
```

For each worktree (except the main repository), gather the following information:

1. **Branch name**: Extract from worktree list
2. **Merge status**: Check if merged to master/main
   ```bash
   cd <worktree-path> && git branch --merged master | grep -q "$(git branch --show-current)" && echo "merged" || echo "not-merged"
   ```
3. **Remote status**: Check if branch exists on origin and if ahead/behind
   ```bash
   cd <worktree-path> && git status -sb
   ```
4. **Uncommitted changes**: Check for dirty files
   ```bash
   cd <worktree-path> && git status --short
   ```
5. **Last commit info**: Get last commit date and message
   ```bash
   cd <worktree-path> && git log -1 --format="%cr|%s|%h"
   ```
6. **Commit count**: Number of commits on this branch
   ```bash
   cd <worktree-path> && git rev-list --count HEAD ^master
   ```
7. **Directory size**: Approximate size
   ```bash
   du -sh <worktree-path>
   ```

### Phase 2: Categorize Worktrees

Based on gathered data, categorize each worktree:

- **🗑️ Safe to delete**: Merged to master/main, pushed, no uncommitted changes
- **⚠️ Probably safe (stale)**: Last activity > 14 days, no uncommitted changes
- **✅ Keep (active)**: Recent activity OR has uncommitted/unpushed work

Sort them: Safe → Probably safe → Keep

### Phase 3: Interactive Review

For EACH worktree, display this formatted summary:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🗂️ <worktree-directory-name>
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

📋 Branch: <branch-name>
📊 Status: <merge-status>, <remote-status>
📅 Last activity: <time-ago>
💾 Size: <directory-size>
🔄 Changes: <uncommitted-status>

📝 Work Summary:
   • <commit-count> commits on this branch
   • Last commit: "<commit-message>" (<time-ago>)
   • First commit: "<first-commit-message>" (<time-ago>)

   Files changed: <stats>
     <top-3-changed-files>
     ...and <N> more

   Recent commits (up to 4):
     <hash> <message>
     <hash> <message>
     <hash> <message>
     <hash> <message>
     [<N> more commits...]

🤔 Recommendation: <Safe to delete | Probably safe | Keep - active work>
```

To get the work summary, use these commands:
```bash
# Get commit count and list
cd <worktree-path> && git log master..HEAD --oneline

# Get first commit on branch
cd <worktree-path> && git log master..HEAD --oneline --reverse | head -1

# Get changed files with stats
cd <worktree-path> && git diff master...HEAD --stat

# Get detailed commits (up to 4)
cd <worktree-path> && git log master..HEAD --oneline --format="%h %s" -4
```

After displaying the summary, prompt the user:
```
What would you like to do?
  [y] Yes, delete this worktree
  [n] No, keep this worktree
  [i] Show full commit history & file changes
  [s] Skip all remaining worktrees
  [q] Quit cleanup now

Your choice:
```

**If user chooses "i"**: Show full details:
```bash
# Full commit history
cd <worktree-path> && git log master..HEAD --format="%h (%ar) %s"

# Full file changes
cd <worktree-path> && git diff master...HEAD --stat
```

Then re-prompt for y/n/s/q.

**If user chooses "y"**: Ask about branch deletion:
```
Also delete the branch?
  1. Keep branch (local & remote)
  2. Delete local branch only
  3. Delete local + remote branch
  [1/2/3]:
```

Then execute the deletion:
```bash
# Delete worktree (run from main repo)
git worktree remove <worktree-path>

# If option 2: Delete local branch only
git branch -D <branch-name>

# If option 3: Delete local + remote branch
git branch -D <branch-name> && git push origin --delete <branch-name>
```

**If user chooses "s"**: Stop reviewing remaining worktrees and jump to Phase 4.

**If user chooses "q"**: Stop immediately and show summary of what was done so far.

### Phase 4: Summary

After reviewing all worktrees (or if stopped early), show:
```
✨ Cleanup Summary
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
✓ Reviewed: <N> worktrees
✓ Deleted: <N> worktrees (freed <size>)
✓ Kept: <N> worktrees
✓ Deleted branches: <N> local, <N> remote
```

### Important Notes:
- NEVER delete anything without explicit user permission
- Always show the work summary before asking for deletion
- If any git command fails, show the error and skip that worktree
- Be patient and show clear, formatted output for each step
- Track deletions as you go for the final summary
