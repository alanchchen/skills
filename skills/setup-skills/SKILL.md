---
name: setup-skills
description: Configure this repo for the planning and implement skills — Plan store, issue tracker, and worktree creation. Run once per repo.
disable-model-invocation: true
---

# Setup Skills

Write the per-repo configuration the other skills read. They describe actions ("write the Plan", "mark the Slice done"); the files written here say how to perform each action in this repo.

- **Plan store** — where Plans live: OpenSpec, GitHub issues, or GitLab issues
- **Issue tracker** — where bugs and incoming requests live: GitHub issues or GitLab issues
- **Worktree** — how this repo creates a worktree, recorded only when it has its own way

Detection happens here and nowhere else. Explore, present what you found with a recommended answer, confirm, then write.

## 1. Explore

Read whatever exists:

- `git remote -v` — GitHub, GitLab (`gitlab.com` or a self-hosted host), or no remote
- `openspec/` at the repo root, and whether `openspec --version` runs
- whether `gh` or `glab` runs and is authenticated (`gh auth status`, `glab auth status`)
- `AGENTS.md` and `CLAUDE.md` at the repo root, and any existing `## Agent skills` block in either
- `docs/agents/` — prior output of this skill
- worktree tooling: scripts or hooks whose name mentions `worktree` (e.g. `.claude/hooks/`, `scripts/`), a `WorktreeCreate` hook in `.claude/settings.json`, a `.worktreeinclude`

## 2. Ask, one section at a time

Summarise what you found. Then take each section in order — one section, one answer — leading with the recommendation so the user can accept it in a word.

**A — Plan store.** Recommend OpenSpec when `openspec/` exists; otherwise the platform of the git remote.

When the chosen store's CLI is missing or unauthenticated (`openspec`, `gh`, `glab`), stop and tell the user what to install or log into. Writing the configuration for a store the agent cannot reach leaves every later skill broken.

With no remote and no `openspec/`, there is no store to offer: tell the user to either initialise OpenSpec or add a GitHub/GitLab remote, and stop.

**B — Issue tracker.** Recommend the platform of the git remote. This is independent of A — OpenSpec holds Plans well and bugs poorly, so a repo commonly pairs an OpenSpec Plan store with a GitHub issue tracker. Same CLI check as A.

**C — Worktree.** Skip this section when exploration found no worktree tooling: the implement skill's default (the agent's native worktree feature, else `git worktree add`) applies. When tooling exists, show what you found and ask whether implement should create worktrees through it, and what it needs (arguments, a branch-name format, setup it performs).

## 3. Confirm

Show drafts of every file you will write and the `## Agent skills` block. Let the user edit before writing.

## 4. Write

Seed `docs/agents/` from the templates in this folder, filling in the repo-specific parts:

- `docs/agents/plan-store.md` from [plan-store-openspec.md](./plan-store-openspec.md), [plan-store-github.md](./plan-store-github.md), or [plan-store-gitlab.md](./plan-store-gitlab.md)
- `docs/agents/issue-tracker.md` from [issue-tracker-github.md](./issue-tracker-github.md) or [issue-tracker-gitlab.md](./issue-tracker-gitlab.md)
- `docs/agents/worktree.md` from [worktree.md](./worktree.md) — only when section C ran and the user chose the repo's tooling

The `## Agent skills` block goes in `AGENTS.md`, which every agent reads. If `AGENTS.md` is missing, create it. If `CLAUDE.md` exists and does not already import `AGENTS.md`, add an `@AGENTS.md` line to it. Update an existing block in place, leaving the surrounding sections untouched.

```markdown
## Agent skills

### Plan store

[one line: where Plans live]. See `docs/agents/plan-store.md`.

### Issue tracker

[one line: where bugs and requests live]. See `docs/agents/issue-tracker.md`.

### Worktree

[one line: how worktrees are created]. See `docs/agents/worktree.md`.
```

Include `### Worktree` only when `docs/agents/worktree.md` was written.

## 5. Done

Done when every file from step 4 exists and the block points at each of them. Tell the user which files were written, that the planning and implement skills now read them, and that `docs/agents/*.md` can be edited directly — re-run this skill only to switch a store or tracker.
