# Mutation coherence

**Priority:** HIGH

## When

A server mutation succeeds but route context, a Query list, or SSR-derived UI remains stale.

## Rule

Invalidate the cache that actually owns the displayed data. `queryClient.invalidateQueries` refreshes Query entries (subject to query activity and configuration); `router.invalidate()` refreshes Router route matches/context. Use both only if both own changed values, for example after login or account switching. Keep mutation success pending through required refetch by returning the invalidation promise. Clearing a browser cache on logout does not invalidate HTTP caches or revoke server credentials.

**Prefer for Query-owned order data:**

```tsx
const client = useQueryClient();
const save = useMutation({
  mutationFn: saveOrder,
  onSuccess: (_saved, input) =>
    client.invalidateQueries({ queryKey: ["orders", input.customerId] }),
});
```

For a mutation that changes auth context, also invalidate the router and remove previous-account private Query entries. Check server authorization regardless of route guards or cached identity.

[Query invalidation](https://tanstack.com/query/latest/docs/framework/react/guides/query-invalidation) · [Router data loading](https://tanstack.com/router/latest/docs/framework/react/guide/data-loading)
