# Mutations and invalidation

**Priority:** HIGH

## When

A create/update/delete succeeds but the UI remains stale, or a form needs pending/error feedback.

## Rule

Use `useMutation` for writes, check `isPending` and `isError`, and refresh only affected query families. Returning the invalidation promise from `onSuccess` keeps the mutation pending until the active refetch completes. If the server returns a complete authoritative resource, `setQueryData` may update a known detail entry directly; invalidate related lists whose membership or ordering may have changed. Use `mutateAsync` for an awaited workflow with explicit error handling.

**Avoid:** `invalidateQueries()` with no filter after every write, or assuming a mutation updates query data automatically.

**Prefer:**

```tsx
const client = useQueryClient();
const save = useMutation({
  mutationFn: updateOrder,
  onSuccess: (_result, variables) =>
    client.invalidateQueries({ queryKey: ["orders", variables.customerId] }),
});
```

Review whether other detail or summary keys also depend on this write. Handle server rejection and duplicate submissions according to the UI's requirements.

[Invalidation from mutations](https://tanstack.com/query/latest/docs/framework/react/guides/invalidations-from-mutations)
