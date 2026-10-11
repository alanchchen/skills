# Plan store: GitHub

Plans for this repo live as GitHub issues: one **parent issue** per Plan, one **sub-issue** per Slice. Use the `gh` CLI; `gh` infers the repo from `git remote -v`.

## Where the Plan contract lives

| Contract item | Location |
|---|---|
| Seams | a `## Seams` section in the parent issue body |
| Acceptance criteria | an `## Acceptance criteria` checklist in the parent issue body |
| Slices | sub-issues of the parent, each with `## What to build` and `## Verification` sections, ordered by their blocking links |

## Plan reference

The parent issue number. The worktree branch slug is `<number>-<kebab-case title>`.

## API notes

The sub-issue and dependency endpoints take an issue's **database id** (`gh api repos/{owner}/{repo}/issues/<n> --jq .id`), never its `#number` or `node_id`.

- Add a sub-issue: `gh api --method POST repos/{owner}/{repo}/issues/<parent>/sub_issues -F sub_issue_id=<child-db-id>`
- List sub-issues: `gh api repos/{owner}/{repo}/issues/<parent>/sub_issues`
- Add a blocking link: `gh api --method POST repos/{owner}/{repo}/issues/<blocked>/dependencies/blocked_by -F issue_id=<blocker-db-id>`
- List blockers: `gh api repos/{owner}/{repo}/issues/<n>/dependencies/blocked_by`

Where sub-issues or dependencies are unavailable, fall back to a task list of `- [ ] #<n>` in the parent body and a `Blocked by: #<n>, #<n>` line at the top of each Slice body.

## When a skill says "write the Plan"

1. Create the parent issue with `gh issue create`, the contract sections in its body (heredoc for multi-line bodies).
2. Create the Slices in dependency order — blockers first — so each blocking link can name a real issue. Attach each as a sub-issue of the parent, then add its blocking links.

## When a skill says "read the Plan"

`gh issue view <parent> --comments`, then each sub-issue's body and state from the sub-issue list.

## When a skill says "check the Plan contract"

The contract holds when all three are true:

- the parent body has a `## Seams` section naming at least one seam
- the parent body has an `## Acceptance criteria` section with at least one item
- the parent has at least one sub-issue, and every sub-issue has a non-empty `## Verification` section

## When a skill says "find the next Slice"

A Slice is **done** when its issue is closed, or a commit on the current branch closes it (`git log <base>..HEAD --grep "Closes #<n>"`) — `Closes #<n>` only closes the issue when the branch merges, so mid-Plan the issue is still open.

The next Slice is the first sub-issue, in parent order, that is not done and whose blockers are all done. None left means the Plan is done.

## When a skill says "mark a Slice done"

End the Slice's commit message with `Closes #<n>`. The issue closes when the branch merges; skills never close it through the CLI.

## When a skill says "amend the Plan"

Edit the affected issue body with `gh issue edit <n> --body-file -`, then `gh issue comment <n>` with what changed and why — the comment is the amendment's history.
