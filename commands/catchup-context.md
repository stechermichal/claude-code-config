Load full project context AND understand current work:

**PROJECT CONTEXT:**
1. Read CLAUDE.md thoroughly - architecture, tech stack, conventions, development guidelines
2. Check package.json for current dependency versions (framework, libraries, etc.)
3. Review database schema (e.g., prisma/schema.prisma) - scan key tables and their relationships
4. Check 2-3 most recent migrations to understand recent schema changes
5. Understand project structure (apps/ vs packages/, src/ layout, etc.)
6. Internalize key patterns: naming conventions, testing strategy, code organization

**CURRENT WORK CONTEXT:**
7. Check current branch name: `git branch --show-current`
8. Review commits on this branch: `git log master..HEAD --oneline` (or main)
9. Read commit messages to understand what's been implemented so far
10. Check scope of changes: `git diff master...HEAD --stat`
11. If there are staged or unstaged changes, review those to see current work in progress
12. Understand: What feature are we working on? What's already done? What's next?

Once loaded, provide a summary:
- Project tech stack and key conventions
- Current branch and feature being worked on
- What's been implemented (from commit history)
- What's currently in progress (from diffs)
- Confirm you're ready to continue where we left off
