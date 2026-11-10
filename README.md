# Coding-agent skills

Portable [Agent Skills](https://agentskills.io/specification) for coding agents. Each skill lives in `skills/<name>/SKILL.md`, with detailed rules in its `rules/` directory.

## Skills

| Skill | Purpose |
| --- | --- |
| [react](skills/react/SKILL.md) | Advanced React development: composition, rendering, identity, effects, async behavior, and performance. |

## Install

Copy a skill directory, including its `rules/`, into a directory your agent scans:

```sh
mkdir -p .agents/skills
cp -R skills/react .agents/skills/
```

Both [OpenCode](https://opencode.ai/v2/docs/skills) and [Cursor](https://cursor.com/docs/skills) discover project skills under `.agents/skills/`. For a global install, use `~/.agents/skills/` instead. Alternatively, OpenCode can load this repository's `skills/` directory via a `skills` entry in its configuration; a plain `skills/` folder at the project root is **not** automatically discovered.
