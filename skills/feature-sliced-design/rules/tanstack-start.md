# TanStack Start integration

**Priority:** HIGH

## When

Organizing TanStack Start applications with TanStack Router file-based routes, loaders, server functions, or server routes.

## Rule

Keep file route definitions in the configured TanStack Router route directory (commonly `src/routes/`), not inside FSD page slices. `__root.tsx`, route files, generated route-tree output, and framework entry points have framework-defined roles; inspect the project's router-plugin configuration before moving them. The FSD `app` layer can own app-wide providers/router setup; page slices own screen-specific UI and reusable behavior lives in lower layers. A route file composes the page public API and keeps route-specific `createFileRoute` configuration such as params/search validation, `beforeLoad`, `loader`, error/not-found components, and server handlers visible to the router.

```text
src/
  routes/                    # TanStack Router / Start route files
    __root.tsx
    products.$productId.tsx
  app/                       # FSD app-wide setup
  pages/product/             # Product page slice
  widgets/  features/  entities/  shared/
```

When route-specific logic grows, delegate it to a page slice but keep ownership of router types and URL behavior clear; avoid making lower layers import `routes/` or generated route-tree internals. A loader can call a page-level data function or lower-layer API through its public API. Route guards are for navigation UX, **not authorization**: enforce authentication, resource permissions, and input validation inside every private server function or server route. Keep server-only data access in `.server.ts` or otherwise protected server modules; client-visible slice exports must not expose secrets or server dependencies. Do not put mutable request data at module scope.

**Check:** regenerate the route tree through the project's usual dev/build command, then test direct visits, client navigation, SSR/hydration, error states, and server endpoint access. Use `tanstack-router` for route behavior, `tanstack-start` for server/runtime rules, and `tanstack-integration` when Query participates in loading.

[TanStack Router file-based routing](https://tanstack.com/router/latest/docs/framework/react/routing/file-based-routing) · [TanStack Start overview](https://tanstack.com/start/latest/docs/framework/react/overview)
