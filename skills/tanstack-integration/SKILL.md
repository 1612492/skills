---
name: tanstack-integration
description: Coordinate TanStack Router loaders with TanStack Query caching, prefetching, mutation invalidation, and TanStack Start SSR hydration. Use when two or more of @tanstack/react-router, @tanstack/react-query, and @tanstack/react-start are used together; prevent duplicate fetching, request cache leaks, and mismatched data ownership.
---

# TanStack integration (React)

This skill is for combining tools, not replacing their individual guidance. Check installed package versions before using APIs: Start's documented SSR Query integration requires compatible Query/Router integration packages, and `queryClient.query` in the current example requires Query 5.102+.

## Workflow

1. Decide which cache owns each value: Router loader cache for simple route data, or Query for shared cross-route server state. Let Router coordinate fetching regardless.
2. Match keys/dependencies in preloading and components, then check direct navigation, client navigation, SSR, hydration, and mutations.
3. Apply the relevant rule and the corresponding `tanstack-query`, `tanstack-router`, or `tanstack-start` skill when implementation detail is needed.

| Task                 | Rule                                                          |
| -------------------- | ------------------------------------------------------------- |
| Loader plus Query    | [Loader and cache ownership](rules/loader-and-cache.md)       |
| Start SSR and Query  | [Request isolation and hydration](rules/request-isolation.md) |
| Writes across caches | [Mutation coherence](rules/mutation-coherence.md)             |

Use the [Router external data loading](https://tanstack.com/router/latest/docs/framework/react/guide/external-data-loading) and [Start Query integration](https://tanstack.com/start/latest/docs/framework/react/guide/tanstack-query) guides as the current source for setup details.
