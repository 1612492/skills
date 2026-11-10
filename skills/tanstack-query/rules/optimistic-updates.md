# Optimistic updates

**Priority:** MEDIUM

## When

A write needs immediate feedback despite network latency.

## Rule

For one local UI, displaying pending mutation `variables` is usually simpler than rewriting the cache. For shared optimistic state, cancel conflicting refetches, snapshot the old entry, update immutably in `onMutate`, restore on error, then reconcile/invalidate after settlement. Concurrent writes may need per-item rollback rather than restoring a whole list snapshot.

**Avoid:** mutating cached objects in place or adding a temporary item with the same key as an existing item.

**Prefer for a single component:**

```tsx
const client = useQueryClient();
const add = useMutation({
  mutationFn: createTask,
  onSettled: () => client.invalidateQueries({ queryKey: ["tasks"] }),
});

// Alongside confirmed tasks, render add.variables while add.isPending.
// On failure, show add.error and offer a retry using the same variables.
```

For shared cache writes, use the current v5 `onMutate`/`onError` callback signatures and test overlapping mutations and failed refetches. Optimistic UI is not evidence that the write succeeded.

[Optimistic updates](https://tanstack.com/query/latest/docs/framework/react/guides/optimistic-updates)
