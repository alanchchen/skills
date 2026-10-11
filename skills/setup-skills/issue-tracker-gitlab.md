# Issue tracker: GitLab

Bugs and incoming requests for this repo live as GitLab issues. Use the [`glab`](https://gitlab.com/gitlab-org/cli) CLI; `glab` infers the repo from `git remote -v`. GitLab calls comments "notes" and PRs "merge requests".

## Conventions

- **Create**: `glab issue create --title "..." --description "..."` (heredoc for multi-line descriptions)
- **Read**: `glab issue view <n> --comments`; `-F json` for machine-readable output
- **List**: `glab issue list -F json`, with `--label` filters
- **Comment**: `glab issue note <n> --message "..."`
- **Labels**: `glab issue update <n> --label "..."` / `--unlabel "..."`
- **Close**: `glab issue close <n>` takes no comment — post the explanation first with `glab issue note`, then close

GitLab numbers issues and merge requests separately, so `#42` is an issue and `!42` a merge request.

## When a skill says "publish to the issue tracker"

Create a GitLab issue.

## When a skill says "fetch the issue"

`glab issue view <n> --comments`
