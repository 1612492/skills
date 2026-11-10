# Freshness and cache lifetime

**Priority:** HIGH

## When

Data refetches on navigation/focus, or stays around longer/shorter than expected.

## Rule

`staleTime` controls when cached data becomes stale and eligible for automatic background refetch; `gcTime` controls how long _inactive_ cache entries are retained. Neither is an HTTP cache policy. Query v5 defaults to immediately stale data and roughly five minutes of inactive cache retention. Pick a `staleTime` based on actual volatility and invalidation strategy; keep `gcTime` long enough for expected navigation or hydration. `Infinity` still allows manual invalidation; `'static'` ignores even invalidation and is only for data that truly cannot change during the session.

**Avoid:** disabling all refetch triggers to mask an incorrect key, or setting `gcTime: 0` to make dynamic data fresh.

**Prefer:**

```tsx
useQuery({
  queryKey: ["catalog", categoryId],
  queryFn: () => getCatalog(categoryId),
  staleTime: 60_000, // Example only; choose based on the product's needs.
});
```

Check active/inactive status, focus/reconnect triggers, SSR hydration, and invalidation before changing a global default. A stale query can display cached data while refetching.

[Important defaults](https://tanstack.com/query/latest/docs/framework/react/guides/important-defaults)
