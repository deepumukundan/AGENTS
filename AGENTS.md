# Agent Rules & Guidelines

## Rule 1: Repository Setup
**When entering a new repository:**
1. Check if repo's AGENTS.md starts with `@~/code/AGENTS/AGENTS.md`
2. If missing, add that include at the very top (enables auto-loading via chain: `CLAUDE.md (Automatic when using claude code) → @AGENTS.md (Most AI tools) → This AGENTS.md`)
3. Finally, use Read tool on `~/code/AGENTS/AGENTS.md` to load rules into current session

## Rule 2: Code Search with ripgrep
**Use `rg` (ripgrep) for all code searches:**
- `rg` is blazing fast, respects .gitignore, and integrates seamlessly with terminals
- Prefer `rg` over grep, `grep_search` tool, or manual file reading for search tasks
- Use semantic_search tool only when pattern-based search won't find what you need
- Examples:
  ```bash
  rg "pattern" --type ts              # Search TypeScript files
  rg "function_name" -A 3 -B 3       # Show context lines
  rg "TODO|FIXME" --no-heading        # Find all TODOs
  ```

## Rule 3: Permissions & Development Standards
**Permissions:** All shell commands allowed. `rm` and any destructive commands require user approval.

**Languages:** Prefer Python, Kotlin, TypeScript, Rust. Prefer TypeScript over JS.
For Python: always use type hints and always try to activate the virtual env before running build and tests.

**Secrets:** Never print unless explicitly asked. Use placeholders (`API_KEY=<redacted>`).

## Rule 4: Planning & Approval
**Before starting work:**
- Ask clarifying questions for ambiguous requirements, architectural decisions, multi-file changes, or unclear scope
- Request approval before working on any issue or installing dependencies:
```
Ready to work on: [Title]
Plan: [bullet points]
Files to modify: [list]
Proceed? [Yes/No]
```
Keep questions focused and actionable. Don't ask about obvious details.

## Rule 5: Git Workflow
**Branching:** NEVER commit directly to master/main/development. Use feature branches + PR.
```bash
git checkout -b ISSUE-ID              # Create branch
git push -u origin ISSUE-ID           # Push to remote
gh pr create                          # Open PR
```
After merge: `git branch -d ISSUE-ID && git push origin --delete ISSUE-ID`

**Commits:** After every stable change set. Single-line messages.
```bash
git commit -m "ISSUE-ID Brief description"
```
Push automatically on feature branches, never on master/main/development without approval.
**Prefer squash** instead of merge.

**Parallel PRs:**
- Start each from latest master/main/development
- Conflicts: keep both lines if different IDs; same ID → newer timestamp wins

## Rule 6: Operational
**Monitoring:** Exponential backoff (5s → 10s → 20s → 40s → 60s cap). Background processes when possible.

**Parallel agents:** Each uses own git worktree: `git worktree add ../REPO-ISSUE-ID -b ISSUE-ID <base-branch>`
