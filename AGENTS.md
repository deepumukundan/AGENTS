# Agent Rules & Guidelines

## Rule 1: Repository Setup
**When entering a new repository:**
1. Run `bd onboard` if `.beads/` doesn't exist
2. After the `bd onboard` stage, check if repo's AGENTS.md starts with `@~/code/AGENTS/AGENTS.md`
3. If missing, add that include at the very top (enables auto-loading via chain: `CLAUDE.md (Automatic when using claude code) → @AGENTS.md (Most AI tools) → This AGENTS.md`)
4. Finally, use Read tool on `~/code/AGENTS/AGENTS.md` to load rules into current session

## Rule 2: Issue Tracking with beads (bd)
**Use bd for ALL task tracking. NEVER use TodoWrite or TODO comments.**

```bash
bd ready                              # Show unblocked work
bd create "Title" -t task -p 1 -d "Description"  # Create issue
bd update ID --status in_progress     # Claim work
bd close ID --reason "Done"           # Complete work
```

**Types:** `bug`, `feature`, `task`, `epic`, `chore`
**Priorities:** `0` (critical) → `4` (backlog)
**Statuses:** `open`, `in_progress`, `blocked`, `closed`

Always commit `.beads/issues.jsonl` with code changes.

## Rule 3: Planning & Approval
**Before starting work:**
- Ask clarifying questions for ambiguous requirements, architectural decisions, multi-file changes, or unclear scope
- Request approval before working on any bead issue or installing dependencies:
```
Ready to work on [ID]: [Title]
Plan: [bullet points]
Files to modify: [list]
Proceed? [Yes/No]
```
Keep questions focused and actionable. Don't ask about obvious details.

## Rule 4: Git Workflow
**Branching:** NEVER commit directly to master/main/development. Use feature branches + PR.
```bash
git checkout -b ISSUE-ID              # Create branch
git push -u origin ISSUE-ID           # Push to remote
gh pr create                          # Open PR
```
After merge: `git branch -d ISSUE-ID && git push origin --delete ISSUE-ID`

**Commits:** After every file change. Single-line messages. Include `.beads/issues.jsonl`.
```bash
git commit -m "Brief description (ISSUE-ID)"
```
Push automatically on feature branches, never on master/main/development without approval.
**Prefer squash** instead of merge.

**Parallel PRs:**
- Start each from latest master/main/development, claim issue, commit code + beads together
- After merge: close issue on the base branch
- Conflicts: keep both lines if different IDs; same ID → newer timestamp wins; run `bd import`

## Rule 5: Development Standards
**Permissions:** All Bash commands allowed. `rm` requires user approval.

**Types:** Prefer Python, Kotlin, TypeScript, Rust. For Python: always use type hints. Prefer TypeScript over JS.

**Secrets:** Never print unless explicitly asked. Use placeholders (`API_KEY=<redacted>`).

## Rule 6: Operational
**Context:** Report usage after every response: `Context: XX% used`

**Monitoring:** Exponential backoff (5s → 10s → 20s → 40s → 60s cap). Background processes when possible.

**Parallel agents:** Each uses own git worktree: `git worktree add ../REPO-ISSUE-ID -b ISSUE-ID <base-branch>`

## Landing the Plane
When user says "land the plane" or "pass the baton":
- Follow the "Landing the Plane" workflow in repo's AGENTS.md (auto-added by `bd init`)
- After completing that workflow, **optionally spawn new agent** with continuation prompt using Task tool
