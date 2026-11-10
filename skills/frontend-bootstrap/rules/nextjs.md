# Next.js setup

**Priority:** HIGH

## When

Bootstrapping the stack in a Next.js application.

## Rule

Inspect whether the app uses App Router, Pages Router, or both; default to the chosen scaffold's convention. Keep `app/` or `pages/` (at root or under `src/`) and all special route files discoverable. For App Router, prefer Server Components for server reads; use TanStack Query where client-side shared cache, background refresh, or mutations justify it. Put the Query provider in a small `"use client"` component rendered by a suitable layout, not a top-level client layout just to add a provider. Create the QueryClient with a request-safe lifecycle: never share private query state across server requests; follow the installed Query version's [Next.js SSR guidance](https://tanstack.com/query/latest/docs/framework/react/guides/advanced-ssr) if prefetched data is hydrated. A client-only Query provider does not automatically hydrate server data.

Install Tailwind/shadcn using their [Next.js guide](https://ui.shadcn.com/docs/installation/next) and the app's existing CSS/alias setup. Do not overwrite global styles or make server-only modules reachable through client-visible public APIs. Use React Hook Form only inside client components; validate sensitive submissions again on the server (route handlers/server actions). With Pages Router, preserve `_app`, `_document`, and existing data-fetching exports; mount a provider where that router expects it.

Use the [Next.js Vitest guide](https://nextjs.org/docs/app/guides/testing/vitest) for a test configuration with React plugin, optional TypeScript path resolution, and `jsdom`. Do not rely on Vitest alone for async Server Components: use integration/E2E tests for those. Verify `next build`, direct routes, hydration, CSS, metadata, and provider boundaries. Avoid adding TanStack Router or TanStack Start packages to a Next.js app solely because Query is present.
