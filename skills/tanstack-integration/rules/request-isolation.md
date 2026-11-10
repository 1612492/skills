# Request isolation and hydration

**Priority:** CRITICAL

## When

Combining Start SSR with Query, hydrating cached data, or debugging cross-user leaks/duplicate initial requests.

## Rule

Create a fresh `QueryClient` inside the router factory that Start calls per server request; use the same instance for the router's context and Query provider. On the browser, keep the client associated with that router. Prefer the installed `@tanstack/react-router-ssr-query` integration for provider wrapping, dehydrate/hydrate, and streaming; do not independently dehydrate the same cache or add another provider/client. Set a suitable Query `staleTime` to avoid immediate redundant refetch after hydration. Query cache isolation is not authorization or protection from a shared CDN cache.

**Prefer (with compatible integration package versions):**

```tsx
export function getRouter() {
  const queryClient = new QueryClient({
    defaultOptions: { queries: { staleTime: 30_000 } },
  });
  const router = createRouter({
    routeTree,
    context: { queryClient },
    defaultPreloadStaleTime: 0,
  });
  setupRouterSsrQueryIntegration({ router, queryClient });
  return router;
}
```

Register `queryClient: QueryClient` in the root route context. Do not put personalized data in public HTTP caches or query keys containing secrets. Test concurrent SSR requests for different accounts, hydration without an unwanted duplicate read, and account changes in the same browser.

[Start + Query](https://tanstack.com/start/latest/docs/framework/react/guide/tanstack-query)
