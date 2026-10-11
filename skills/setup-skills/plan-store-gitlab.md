# Plan store: GitLab

Plans for this repo live as GitLab issues: one **parent issue** per Plan, one **child issue** per Slice. Use the [`glab`](https://gitlab.com/gitlab-org/cli) CLI; `glab` infers the repo from `git remote -v`. GitLab calls comments "notes".

## Where the Plan contract lives

| Contract item | Location |
|---|---|
| Seams | a `## Seams` section in the parent issue description |
| Acceptance criteria | an `## Acceptance criteria` checklist in the parent issue description |
| Slices | child issues, each with `## What to build` and `## Verification` sections, listed in order as a `- [ ] #<n>` task list under `## Slices` in the parent description |

## Plan reference

The parent issue number. The worktree branch slug is `<number>-<kebab-case title>`.

## Linking

- **Parent ↔ child**: `Part of #<parent>` as the first line of each child description, plus the parent's `## Slices` task list. The task list is the Slice order.
- **Blocking**: the native blocking link, posted as a quick action — `glab issue note <child> --message "/blocked_by #<blocker>"`. Native blocking links need GitLab Premium or Ultimate; elsewhere use a `Blocked by: #<n>, #<n>` line under `Part of`. Read native links with `glab api projects/:id/issues/<n>/links`.

## When a skill says "write the Plan"

1. Create the parent issue with `glab issue create --title "..." --description "..."` (heredoc for multi-line descriptions), the contract sections in its description.
2. Create the Slices in dependency order — blockers first — so each blocking link can name a real issue. Link each to the parent and add its blocking links.
3. Fill the parent's `## Slices` task list with the child numbers in order (`glab issue update <parent> --description "..."`).

## When a skill says "read the Plan"

`glab issue view <parent> --comments`, then each child listed under `## Slices`.

## When a skill says "check the Plan contract"

The contract holds when all three are true:

- the parent description has a `## Seams` section naming at least one seam
- the parent description has an `## Acceptance criteria` section with at least one item
- the parent's `## Slices` lists at least one child, and every child has a non-empty `## Verification` section

## When a skill says "find the next Slice"

A Slice is **done** when its issue is closed, or a commit on the current branch closes it (`git log <base>..HEAD --grep "Closes #<n>"`) — `Closes #<n>` only closes the issue when the branch merges, so mid-Plan the issue is still open.

The next Slice is the first child in `## Slices` order that is not done and whose blockers are all done. None left means the Plan is done.

## When a skill says "mark a Slice done"

End the Slice's commit message with `Closes #<n>`. The issue closes when the branch merges into the default branch; skills never close it through the CLI.

## When a skill says "amend the Plan"

Edit the affected description with `glab issue update <n> --description "..."`, then `glab issue note <n> --message "..."` with what changed and why — the note is the amendment's history.
