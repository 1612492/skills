# Next.js integration

**Priority:** HIGH

## When

Organizing a Next.js App Router or Pages Router project without breaking framework routing or server/client behavior.

## Rule

Keep Next.js route directories (`app/` for App Router, `pages/` for Pages Router) where Next.js discovers them, at the project root or under `src/` according to the target project. Keep FSD layers separate, named `_app` and `_pages` **for both router styles** so neither is mistaken for a Next.js route directory. Placing framework routes at the root and FSD code in `src/` is a convenient option for a new project, not a reason to move existing routes.

```text
app/                        # Next.js App Router (framework-owned)
  products/[id]/page.tsx
  api/products/route.ts
src/
  _app/                     # FSD app-wide setup / optional API handler composition
  _pages/product/           # Product page slice
  widgets/  features/  entities/  shared/
```

Route files can re-export a page from `src/_pages/product` when the signatures and server/client boundaries match; otherwise keep a small route adapter for params, metadata, layout, loading, error, and HTTP handlers. Preserve Next.js special filenames and directives in the files where Next.js needs them. Route handlers under `app/api/**/route.ts` (or `pages/api/**` for Pages Router) must remain discoverable; they may delegate implementation to suitable server-only modules, with authentication and validation enforced at the endpoint. Keep middleware/proxy and instrumentation in the location required by the installed Next.js version.

In App Router, Server Components are the default; put `"use client"` only at the required client boundary and never pull `server-only` modules, secrets, or database clients into client-visible public APIs. If a slice needs both runtimes, split exports (for example `index.ts` for client-safe code and `index.server.ts` for server-only code) when necessary. Keep caching and revalidation near their data owner; choose page-level data composition when it is not reused. For Pages Router, preserve `_app`, `_document`, data-fetching exports, and API route conventions.

**Check:** build the app, visit routes directly and through navigation, and verify metadata, loading/error behavior, and server/client bundling. Use the installed Next.js version's project-structure documentation for special-file behavior.

[FSD with Next.js](https://feature-sliced.design/docs/guides/tech/with-nextjs) · [Next.js project structure](https://nextjs.org/docs/app/getting-started/project-structure)
