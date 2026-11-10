# Scaffold and structure

**Priority:** HIGH

## When

Creating a new Next.js or TanStack Start app, or adopting the stack in an existing repository.

## Rule

For a **new app**, agree on target directory, framework, package manager, TypeScript, source-root choice, and deployment requirements. Use the official [create-next-app](https://nextjs.org/docs/app/getting-started/installation) or [TanStack Start quickstart](https://tanstack.com/start/latest/docs/framework/react/quick-start), whichever matches the chosen framework. Let the generator set up its framework files, then inspect its package versions, scripts, Tailwind configuration, aliases, and route tree before adding anything else. Do not run a generator into an existing nonempty directory unless the user explicitly approves the effect.

For an **existing app**, inventory installed packages and lockfiles, keep its package manager, and work within its workspace package. Do not create a second router, stylesheet, provider, formatter, test runner, or lockfile. If moving from ESLint/Prettier or another setup, first assess incompatible rules/plugins and ask before replacing it; an additive Oxlint/Oxfmt trial is safer than silently removing working checks. Preserve user-authored config and dirty changes.

Structure around actual business areas: a thin framework route delegates UI/data composition to an owning page or feature module. A pragmatic feature-first start may use `src/features/<area>/{components,hooks,queries,server,schemas,types,utils}/` as needed, `src/components/ui/` for shared shadcn primitives, and a focused `src/lib/` for general infrastructure. Avoid reorganization when adding one dependency to a mature app.

Place validation by its owner; keep secrets and privileged reads in server-only functions/modules. Preserve existing route behavior and import aliases. Add a minimal example only when it demonstrates real integration (such as a form or data screen); do not generate a large demo domain or placeholders throughout the app.
