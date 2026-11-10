# Query identity

**Priority:** CRITICAL

## When

Cache entries collide, filtered results show the wrong data, or a query does not update when its inputs change.

## Rule

Use a top-level array key whose serializable parts contain every variable that changes the fetched resource. Keep the key and query function together with `queryOptions` when multiple callers need them. Object property order is normalized in a query key; array order is not. Do not put credentials or secrets in a key, or reuse a key between `useQuery` and `useInfiniteQuery` (their cached data shapes differ).

**Avoid:** `queryKey: ['orders']` when `customerId` and `status` affect the request.

**Prefer:**

```tsx
const ordersOptions = (customerId: string, status: string) =>
  queryOptions({
    queryKey: ["orders", customerId, { status }],
    queryFn: () => fetchOrders({ customerId, status }),
  });

const orders = useQuery(ordersOptions(customerId, status));
```

For tenant-specific data, include the tenant identity when appropriate, but still authorize the request server-side. Keys identify cached data; they are not access control.

[Query keys](https://tanstack.com/query/latest/docs/framework/react/guides/query-keys)
