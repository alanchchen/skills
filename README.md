# Skills

Personal development skills, from idea to commit, usable across repos and agents.

| Skill | Invocation | What it does |
|---|---|---|
| [setup-skills](./skills/setup-skills/SKILL.md) | user | Configure a repo's Plan store, issue tracker, and worktree creation. Run once per repo. |

## Install

Claude Code:

```
/plugin install alanchchen-skills --marketplace alanchchen/skills
```

Codex and other agents:

```bash
npx skills@latest add alanchchen/skills
```

Then run `setup-skills` once per repo.

Vocabulary lives in [CONTEXT.md](./CONTEXT.md); decisions in [docs/adr/](./docs/adr/).

## Credits

The workflow and several skills are adapted from [mattpocock/skills](https://github.com/mattpocock/skills) (MIT, © Matt Pocock).
