# Server functions

**Priority:** HIGH

## When

Calling a server-side operation from a Start route loader, component, or event handler.

## Rule

Use `createServerFn` for app-internal RPC. Choose `GET` for reads and `POST` for writes; validate data at runtime on the server, not only with TypeScript. Keep sensitive helpers in server-only modules and invoke server functions using their `{ data }` argument shape. Inputs and outputs cross a serialization boundary; do not return private fields unnecessarily. For endpoints used by external consumers, use a server route instead.

**Prefer:**

```tsx
// inventory.functions.ts; getInventoryItem is a server-only helper.
const itemInput = z.object({ sku: z.string().min(1) });

export const getItem = createServerFn({ method: "GET" })
  .validator(itemInput)
  .handler(async ({ data }) => getInventoryItem(data.sku));

// In a route loader:
const item = await getItem({ data: { sku: params.sku } });
```

Validation alone does not authorize access to an item. Do not dynamically import a server function from a client component; use static imports so Start can generate RPC stubs.

[Server functions](https://tanstack.com/start/latest/docs/framework/react/guide/server-functions)
