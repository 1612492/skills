# Loaders and dependencies

**Priority:** HIGH

## When

Fetching route data, preloading links, or debugging stale/wrong loader results.

## Rule

Load critical data in `loader` rather than waiting for a mounted component to start fetching. Use `params` and selected `loaderDeps` as the loader's data identity; pass `abortController.signal` when the data source supports it. Router's `staleTime`, `preloadStaleTime`, and `gcTime` are for its _loader cache_, not Query's cache. `beforeLoad` executes serially along the matched route hierarchy; loaders can then run in parallel. Avoid unnecessary network work in `beforeLoad`.

**Prefer:**

```tsx
export const Route = createFileRoute("/teams/$teamId")({
  loader: ({ params, abortController }) =>
    loadTeam(params.teamId, { signal: abortController.signal }),
  component: TeamPage,
});

function TeamPage() {
  const team = Route.useLoaderData();
  return <h1>{team.name}</h1>;
}
```

For shared cross-route data with mutations/background refreshes, use the existing Query cache and its policies instead. Verify preload and navigation both receive the right entity and no unhandled promise escapes.

[Data loading](https://tanstack.com/router/latest/docs/framework/react/guide/data-loading)
