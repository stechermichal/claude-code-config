## Instructions

You will create a new git worktree for the current project based on a GitHub issue.

### Step 1: Verify main repo is on master/main and up to date
Before fetching the issue, check that the repository is on master (or main) and pull latest:

```bash
git branch --show-current
```

If NOT on master/main:
- Alert the user: "⚠️ Repository is currently on branch '<branch-name>', not master/main. Please switch to the main branch first or confirm you want to create the worktree from '<branch-name>'."
- Ask if they want to proceed anyway or abort

If on master/main, pull latest changes:
```bash
git pull
```

If pull fails (merge conflicts, uncommitted changes, etc.):
- Show the error
- Alert: "⚠️ Cannot pull latest changes. Please resolve issues first."
- Abort the worktree creation

### Step 2: Fetch the issue
Run `gh issue view $ARGUMENTS` to get the issue details (title, labels, body).

```bash
gh issue view $ARGUMENTS
```

### Step 3: Analyze and generate branch name
Based on the issue details, generate a branch name following this pattern:
```
<area>/<type>/<descriptive-name>
```

Where:
- **area**: Determine from issue labels or content (e.g., `app`, `admin`, `server`, `api`, `web`, `docs`)
  - If unclear, infer from the issue description or ask the user

- **type**: One of: `feature`, `fix`, `chore`, `update`, or `test`
  - `feature`: New functionality or enhancements
  - `fix`: Bug fixes
  - `chore`: Maintenance, refactoring, deps updates
  - `update`: System or major updates
  - `test`: Test-related work

- **descriptive-name**: Short kebab-case description (2-4 words max)
  - Extract key terms from issue title
  - Keep it concise and meaningful
  - Example: `date-picker-qol`, `auth-token-expiry`, `update-dependencies`

### Step 4: Present to user
Show the proposed branch name and ask for confirmation:
```
📋 Issue #<number>: "<title>"
Labels: <labels>

Proposed branch name: <generated-branch-name>

Proceed? (y/n)
```

If user says no or suggests changes, adjust the branch name accordingly.

### Step 5: Create the worktree
Once confirmed, create the worktree. The typical command is:
```bash
git worktree add ../<project-name>-<branch-name> -b <branch-name>
```

Or if your project has a setup script:
```bash
./setup-worktree.sh <branch-name>
```

### Important notes:
- NEVER create the worktree without user confirmation
- If the issue doesn't exist or `gh` fails, explain the error clearly
- If you can't determine the area, ask the user
- Run commands from the main project directory
