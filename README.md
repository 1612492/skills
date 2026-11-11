# Coding-agent skills

Portable [Agent Skills](https://agentskills.io/specification) for coding agents. Each skill lives in `skills/<name>/SKILL.md`, with detailed rules in its `rules/` directory.

## Skills

| Skill                                                        | Purpose                                                                                                   |
| ------------------------------------------------------------ | --------------------------------------------------------------------------------------------------------- |
| [react](skills/react/SKILL.md)                               | Advanced React development: composition, rendering, identity, effects, async behavior, and performance.   |
| [tanstack-query](skills/tanstack-query/SKILL.md)             | Query keys, freshness, mutations, optimistic updates, and pagination.                                     |
| [tanstack-router](skills/tanstack-router/SKILL.md)           | Typed routes, validated search, loaders, navigation, and route boundaries.                                |
| [tanstack-start](skills/tanstack-start/SKILL.md)             | Server functions, security, middleware, SSR, and server routes.                                           |
| [tanstack-integration](skills/tanstack-integration/SKILL.md) | Coordinate Router, Query, and optionally Start without duplicate caches.                                  |
| [frontend-bootstrap](skills/frontend-bootstrap/SKILL.md)     | Scaffold or configure Next.js and TanStack Start apps with UI, data, forms, quality tools, and AGENTS.md. |
| [commit](skills/commit/SKILL.md)                             | Draft commit messages from changes using commit history or Conventional Commits; commit only on request.  |

## Install

Copy a skill directory, including its `rules/`, into a directory your agent scans:

```sh
mkdir -p .agents/skills
cp -R skills/react skills/tanstack-* skills/frontend-bootstrap skills/commit .agents/skills/
```

Both [OpenCode](https://opencode.ai/v2/docs/skills) and [Cursor](https://cursor.com/docs/skills) discover project skills under `.agents/skills/`. For a global install, use `~/.agents/skills/` instead. Alternatively, OpenCode can load this repository's `skills/` directory via a `skills` entry in its configuration; a plain `skills/` folder at the project root is **not** automatically discovered.

## Formatting

Install dependencies with `pnpm install`, then use `pnpm format` to format every Markdown file or `pnpm format:check` to verify formatting without changing files.
