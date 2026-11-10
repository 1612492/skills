---
name: frontend-bootstrap
description: Create or configure a Next.js or TanStack Start React repository with Tailwind CSS, shadcn/ui, TanStack Query, React Hook Form, Zod, Oxlint, Oxfmt, Vitest, and a project-specific AGENTS.md verification checklist. Use for new frontend app setup or incremental adoption in an existing app; inspect versions and existing config before installing or editing.
---

# Bootstrap a frontend repository

Use this skill for **both new apps and existing repositories**. The default stack is TypeScript, Tailwind CSS, shadcn/ui, TanStack Query, React Hook Form + Zod, Oxlint, Oxfmt, and Vitest + React Testing Library. Adapt choices to the target project instead of installing duplicate or incompatible tooling. This is an agent workflow, not a script that blindly generates files.

## Workflow

1. Inspect the target repository: framework and version, router, package manager/lockfile, workspace root, `src` placement, existing dependencies, scripts, providers, CSS, path aliases, test setup, `AGENTS.md`, and working-tree changes. Ask before choosing a framework or overwriting anything substantial. For a new app, pick an explicit destination; use the framework's current scaffold, not a hand-built approximation.
2. Follow the relevant framework guide, then add only missing stack pieces. Prefer official installers for Tailwind and shadcn; confirm prompts and generated changes before proceeding. Preserve existing styles, lint rules, providers, and tests in an existing repo.
3. Keep framework-required routes and entry files in place. Start with a small feature-first structure and thin route files. Do not scaffold unused layers or empty directories merely to match a diagram.
4. Add meaningful smoke tests and project-specific scripts. Create or carefully update the target repo's `AGENTS.md` with its actual conventions and runnable verification steps; never paste an unverified generic checklist as binding policy.
5. Install, run the checks, inspect the diff, and report failures and skipped checks. Do not describe a generated app as ready until its tests and build have passed.

| Task                                                      | Read                                                                |
| --------------------------------------------------------- | ------------------------------------------------------------------- |
| New vs existing repo, architecture and dependency choices | [Scaffold and structure](rules/scaffold-and-structure.md)           |
| Tailwind, shadcn, forms, lint, format, testing            | [Stack and quality tools](rules/stack-and-quality.md)               |
| Next.js providers, routing, and tests                     | [Next.js setup](rules/nextjs.md)                                    |
| TanStack Start Query/SSR integration and tests            | [TanStack Start setup](rules/tanstack-start.md)                     |
| Generate or update `AGENTS.md` and verify                 | [Agent guidance and verification](rules/agents-and-verification.md) |

Apply `tanstack-query`, `tanstack-router`, `tanstack-start`, and `tanstack-integration` when implementation details exceed bootstrap setup. Check current official framework/tool docs for commands and compatibility rather than pinning versions from this skill.
