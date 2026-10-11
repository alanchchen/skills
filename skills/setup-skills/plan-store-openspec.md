# Plan store: OpenSpec

Plans for this repo live as OpenSpec changes under `openspec/changes/`, committed with the code. Use the `openspec` CLI for every operation it covers.

## Where the Plan contract lives

| Contract item | Location in the change |
|---|---|
| Seams | a `## Seams` section in `design.md` |
| Acceptance criteria | the `#### Scenario:` blocks in the `specs/` deltas |
| Slices | the `## N.` sections of `tasks.md`, in dependency order — each a vertical slice ending in a `**Verification:**` block |

## Plan reference

The change name (kebab-case). It is also the slug for the worktree branch. When the user names no change, run `openspec list --json`; pick the only active change, or ask when there are several.

## When a skill says "write the Plan"

1. `openspec new change "<name>"`
2. `openspec status --change "<name>" --json` — build each artifact in dependency order until every artifact in `applyRequires` is `done`.
3. For each artifact, `openspec instructions <artifact-id> --change "<name>" --json` and write the file at `outputPath` from its `template`. The `context` and `rules` fields constrain what you write; they never appear in the file.
4. Place each contract item where the table above says, on top of the template's own sections.
5. `openspec validate "<name>" --strict` passes.

## When a skill says "read the Plan"

`openspec instructions apply --change "<name>" --json` — read every file in `contextFiles`. Its progress fields report how many tasks are checked.

## When a skill says "check the Plan contract"

The contract holds when all three are true:

- `design.md` has a `## Seams` section naming at least one seam
- `specs/` holds at least one `#### Scenario:`
- every `## N.` section of `tasks.md` ends in a non-empty `**Verification:**` block

## When a skill says "find the next Slice"

The first `## N.` section of `tasks.md` with an unchecked `- [ ]`. None left means the Plan is done.

## When a skill says "mark a Slice done"

Check every `- [ ]` in that `## N.` section, and stage `tasks.md` in the same commit as the Slice's code — a revert of the Slice then reverts its status too.

## When a skill says "amend the Plan"

Edit the affected artifacts, re-run `openspec validate "<name>" --strict`, and commit the artifact edits alone, separate from any code, with a message saying what changed in the Plan and why.

## Gotcha: archiving

`openspec archive` applies a `## MODIFIED Requirements` delta by replacing the whole requirement in the main spec. A delta written before another change was archived silently drops the scenarios that change added, and `validate --strict` still passes. After every archive, check the diff of `###` / `####` heading lines and confirm each removed scenario was meant to go.
