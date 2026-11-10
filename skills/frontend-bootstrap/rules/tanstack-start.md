# TanStack Start setup

**Priority:** HIGH

## When

Bootstrapping the stack in a TanStack Start React application.

## Rule

Use the installed Start/Router version's starter and routing convention. Preserve the configured route directory (commonly `src/routes/`), `__root.tsx`, `src/router.tsx`, generated route tree, `src/start.ts` if present, and bundler plugin registration. Let route loaders coordinate first-render fetching, while TanStack Query owns shared cached server state and mutation invalidation. Route loaders may run in the browser on navigation, so privileged operations must be behind validated, authorized server functions or server routes.

For Query-backed SSR, install compatible `@tanstack/react-query` and `@tanstack/react-router-ssr-query`, then follow the [Start Query integration guide](https://tanstack.com/start/latest/docs/framework/react/guide/tanstack-query). Create a **new QueryClient in the router factory for each SSR request**, pass that same instance in router context, and call `setupRouterSsrQueryIntegration({ router, queryClient })`. Register its type in root route context. The integration wraps the provider and handles hydration/streaming; do not add a second provider or another independent dehydrate/hydrate path. Reuse one query-options factory for loader prefetch and component subscription; include params/search in both loader dependencies and query keys as needed. Set an intentional freshness window, not a universal magic value.

Use the [TanStack Start shadcn guide](https://ui.shadcn.com/docs/installation/tanstack) to place Tailwind/CSS and UI primitives without replacing existing router or styling setup. Configure Vitest with the project's Vite plugin, React plugin, aliases, and DOM test environment; keep Vitest config separate from production Vite/Start config unless the project intentionally shares it. Mock only true external boundaries in unit tests. Verify build and a direct route visit as well as a client transition and hydration; for private data, verify endpoint auth and isolation of two concurrent SSR requests. Consult `tanstack-integration` and `tanstack-start` for the detailed cache and security rules.
