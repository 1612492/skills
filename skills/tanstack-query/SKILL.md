---
name: tanstack-query
description: "Implement and review TanStack Query for React server state: query keys, staleTime and gcTime, mutations, invalidation, optimistic updates, and infinite queries. Use when working with @tanstack/react-query, useQuery, useMutation, queryOptions, or QueryClient. For Router or Start coordination, also use tanstack-integration."
---

# TanStack Query (React)

Use the installed version and the project's existing conventions. The rules target Query v5; check the [current React docs](https://tanstack.com/query/latest/docs/framework/react/overview) before applying version-sensitive APIs. Query manages remote state, not every local UI state variable.

## Workflow

1. Identify which data is server-owned, which inputs identify it, and which writes can change it.
2. Choose the relevant rule below. Keep query options/key definitions shared where loaders, prefetching, and components consume the same data.
3. Test switching parameters, stale/fresh cache behavior, failed writes, and unmount/remount. Inspect the network tab rather than assuming each render fetches.

| Task                                       | Rule                                               |
| ------------------------------------------ | -------------------------------------------------- |
| Wrong cached results or duplicate requests | [Query identity](rules/query-identity.md)          |
| Unexpected refetch or outdated data        | [Freshness and cache lifetime](rules/freshness.md) |
| Writes and cache coherence                 | [Mutations and invalidation](rules/mutations.md)   |
| Immediate feedback before a write finishes | [Optimistic updates](rules/optimistic-updates.md)  |
| Cursor pagination or infinite scroll       | [Infinite queries](rules/infinite-queries.md)      |

Use Router/Start-specific loaders, request isolation, and hydration guidance from `tanstack-integration` when those packages are present. Avoid copying configuration defaults across apps without a freshness requirement.
