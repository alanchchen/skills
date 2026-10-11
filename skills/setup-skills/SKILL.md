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
- `AGENTS.md` and `CLAUDE.md` at the repo root, and whether either already points at `.agents/workflow/README.md`
- `.agents/workflow/` — prior output of this skill (everything else under `.agents/` belongs to other tools)
- worktree tooling: scripts or hooks whose name mentions `worktree` (e.g. `.claude/hooks/`, `scripts/`), a `WorktreeCreate` hook in `.claude/settings.json`, a `.worktreeinclude`

## 2. Ask, one section at a time

Summarise what you found. Then take each section in order — one section, one answer — leading with the recommendation so the user can accept it in a word.

**A — Plan store.** Recommend OpenSpec when `openspec/` exists; otherwise the platform of the git remote.

When the chosen store's CLI is missing or unauthenticated (`openspec`, `gh`, `glab`), stop and tell the user what to install or log into. Writing the configuration for a store the agent cannot reach leaves every later skill broken.

With no remote and no `openspec/`, there is no store to offer: tell the user to either initialise OpenSpec or add a GitHub/GitLab remote, and stop.

**B — Issue tracker.** Recommend the platform of the git remote. This is independent of A — OpenSpec holds Plans well and bugs poorly, so a repo commonly pairs an OpenSpec Plan store with a GitHub issue tracker. Same CLI check as A.

**C — Worktree.** Skip this section when exploration found no worktree tooling: the implement skill's default (the agent's native worktree feature, else `git worktree add`) applies. When tooling exists, show what you found and ask whether implement should create worktrees through it, and what it needs (arguments, a branch-name format, setup it performs).

## 3. Confirm

Show drafts of every file you will write and the pointer line. Let the user edit before writing.

## 4. Write

Seed `.agents/workflow/` from the templates in this folder, filling in the repo-specific parts:

- `.agents/workflow/plan-store.md` from [plan-store-openspec.md](./plan-store-openspec.md), [plan-store-github.md](./plan-store-github.md), or [plan-store-gitlab.md](./plan-store-gitlab.md)
- `.agents/workflow/issue-tracker.md` from [issue-tracker-github.md](./issue-tracker-github.md) or [issue-tracker-gitlab.md](./issue-tracker-gitlab.md)
- `.agents/workflow/worktree.md` from [worktree.md](./worktree.md) — only when section C ran and the user chose the repo's tooling

Then write `.agents/workflow/README.md`, the index — one line per file written:

```markdown
# Workflow

How this repo keeps Plans, tracks issues, and creates worktrees. Written by `setup-skills`; edit freely.

- [plan-store.md](./plan-store.md) — [one line: where Plans live]
- [issue-tracker.md](./issue-tracker.md) — [one line: where bugs and requests live]
- [worktree.md](./worktree.md) — [one line: how worktrees are created]
```

Finally add the pointer line, once, to the repo's agent instructions file — `AGENTS.md` if it exists, else `CLAUDE.md` if it exists, else a new `AGENTS.md`. When a pointer to `.agents/workflow/README.md` is already there, leave it.

```markdown
Before planning, implementing, or filing issues, read `.agents/workflow/README.md` — where this repo keeps Plans, tracks issues, and creates worktrees.
```

## 5. Done

Done when every file from step 4 exists, the README lists each of them, and the pointer line is in place. Tell the user which files were written, that the planning and implement skills now read them, and that `.agents/workflow/*.md` can be edited directly — re-run this skill only to switch a store or tracker.

Ask the user to commit `.agents/workflow/` along with the edited `AGENTS.md` / `CLAUDE.md`. Implement runs in a fresh worktree, which holds only committed files — uncommitted configuration is invisible there, and implement stops for missing setup.
