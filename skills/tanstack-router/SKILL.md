---
name: tanstack-router
description: Build and debug TanStack Router for React routes, file-based routing, typed navigation, validated URL search params, loaderDeps, preloading, redirects, and error boundaries. Use when working with @tanstack/react-router or route files; for Query caching or TanStack Start SSR integration also use tanstack-integration.
---

# TanStack Router (React)

Use the project's installed version and file/code-based route convention. Examples target current Router v1; consult the [React Router documentation](https://tanstack.com/router/latest/docs/framework/react/overview) for version-specific APIs.

## Workflow

1. Inspect the generated route tree or existing code-based tree and router registration before changing a route path.
2. Distinguish route identity (path params), shareable URL state (search), loading dependencies (`loaderDeps`), and transient component state.
3. Choose the matching rule. Verify direct URL entry, client navigation, refresh, malformed search, and failed loading.

| Task                                       | Rule                                                          |
| ------------------------------------------ | ------------------------------------------------------------- |
| Adding/changing a route or navigation      | [Typed routes and navigation](rules/routes-and-navigation.md) |
| Filters, pagination, or URL state          | [Validated search](rules/search-params.md)                    |
| Loading, preloading, or cache mismatch     | [Loaders and dependencies](rules/loaders.md)                  |
| Access control UX, not-found, and failures | [Route boundaries](rules/route-boundaries.md)                 |

Router's built-in loader cache may be enough for simple route data. When Query owns shared server state, let Router coordinate it rather than creating duplicate independent sources of truth; see `tanstack-integration`.
