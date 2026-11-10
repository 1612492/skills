# Stack and quality tools

**Priority:** HIGH

## When

Installing UI styling, forms, data cache, linting, formatting, or unit/component testing.

## Rule

Use the target repo's package manager (examples below use pnpm) and check for existing versions before adding packages. Follow the current framework-specific [Tailwind installation](https://tailwindcss.com/docs/installation) and [shadcn installation](https://ui.shadcn.com/docs/installation) instructions; existing projects may already have Tailwind configured. For shadcn, confirm the chosen framework/template, style, CSS entry, alias, `components.json`, and destination for generated UI primitives; `pnpm dlx shadcn@latest init` is an interactive starting point, not permission to overwrite CSS or components. Add a UI component only if the app needs it.

- Install `@tanstack/react-query`, `react-hook-form`, `zod`, and `@hookform/resolvers` as app dependencies if missing. Initialize Query once per appropriate runtime (see framework rules), and use `zodResolver` for feature-colocated form schemas. Do not create a server-data query for static/local state or put privileged reads into browser functions.
- Add `oxlint` and `oxfmt` as development dependencies. For a newly scaffolded repo, a reasonable starting point is `"lint": "oxlint"`, `"format": "oxfmt"`, and `"format:check": "oxfmt --check"`; use the project's actual scripts when they already exist. Check generated/build outputs are ignored; keep any framework-specific lint checks that Oxlint does not replace. Do not reformat the entire existing repo without agreement.
- Add `vitest`, `@testing-library/react`, `@testing-library/dom`, `@testing-library/user-event`, `@testing-library/jest-dom`, and `jsdom` as development dependencies for React unit/component tests. Add `@vitejs/plugin-react` and `vite-tsconfig-paths` where the framework's Vitest setup calls for them. Configure `jsdom` for DOM tests, a setup file for jest-dom matchers/cleanup if used, and a non-watch CI script such as `"test": "vitest run"` (optionally `"test:watch": "vitest"`). Do not make every server-only test run in jsdom if it needs a Node environment.
- Write at least one useful smoke test for a pure function or client component, and run it. Test validated form behavior with user interactions when a form is added. Use integration or E2E tests for framework-only server behavior; Vitest alone does not prove SSR, route navigation, or security works.

For pnpm, typical missing packages can be added with `pnpm add @tanstack/react-query react-hook-form zod @hookform/resolvers` and `pnpm add -D oxlint oxfmt vitest @testing-library/react @testing-library/dom @testing-library/user-event @testing-library/jest-dom jsdom`; install only what is absent and add framework-specific test dependencies when needed. Check versions and avoid assuming the newest versions are compatible with the scaffold.

[Oxlint setup](https://oxc.rs/docs/guide/usage/linter) · [Oxfmt setup](https://oxc.rs/docs/guide/usage/formatter) · [Vitest guide](https://vitest.dev/guide/) · [React Hook Form and shadcn](https://ui.shadcn.com/docs/forms/react-hook-form)
