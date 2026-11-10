# Loader and cache ownership

**Priority:** CRITICAL

## When

Preloading route data with Query or seeing one response from a loader and a different one in `useQuery`.

## Rule

Use Router's loader to start critical requests before component mount and Query to own the cached value if it is shared or mutated. Define one `queryOptions` factory and use it in both places; include path/search inputs in the Query key, and search values used by the loader in `loaderDeps`. Prefer `ensureQueryData` to guarantee missing critical data, or `prefetchQuery` when the component can show a loading fallback; use the API supported by the installed Query version. When Query controls freshness, consider Router's documented `defaultPreloadStaleTime: 0` so settled Router preloads defer freshness decisions to Query.

**Prefer:**

```tsx
const itemOptions = (id: string) =>
  queryOptions({
    queryKey: ["items", id],
    queryFn: () => getItem(id),
    staleTime: 30_000, // Example freshness window; tailor to the data.
  });

export const Route = createFileRoute("/items/$id")({
  loader: ({ context, params }) => context.queryClient.ensureQueryData(itemOptions(params.id)),
  component: ItemPage,
});

function ItemPage() {
  const { id } = Route.useParams();
  const { data } = useSuspenseQuery(itemOptions(id));
  return <h1>{data.name}</h1>;
}
```

The root route and router must provide the same `queryClient` via typed context and provider. Avoid a module-global client in SSR; see [request isolation](request-isolation.md).

[External data loading](https://tanstack.com/router/latest/docs/framework/react/guide/external-data-loading)
