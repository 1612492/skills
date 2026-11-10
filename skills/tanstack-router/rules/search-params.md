# Validated search

**Priority:** HIGH

## When

Adding bookmarkable pagination, filters, sorting, or search-driven route loading.

## Rule

Search is user-controlled input even if Router parses JSON-like values. Validate it at the route boundary, with safe defaults for recoverable malformed values. Update search through typed `Link` or `navigate`; use functional updates so unrelated search fields survive. When a loader uses a search value, also include _only that value_ in `loaderDeps`. Check the project's schema library/version before picking its adapter.

**Prefer:**

```tsx
export const Route = createFileRoute("/products")({
  validateSearch: (search: Record<string, unknown>) => ({
    page:
      typeof search.page === "number" && Number.isSafeInteger(search.page) && search.page > 0
        ? search.page
        : 1,
  }),
  loaderDeps: ({ search: { page } }) => ({ page }),
  loader: ({ deps: { page } }) => getProducts(page),
  component: Products,
});

function Products() {
  const { page } = Route.useSearch();
  return (
    <Link from={Route.fullPath} search={(old) => ({ ...old, page: page + 1 })}>
      Next
    </Link>
  );
}
```

Do not trust a TypeScript cast (`search as MySearch`) as runtime validation. Clamp unreasonably large page sizes at the boundary.

[Search params](https://tanstack.com/router/latest/docs/framework/react/guide/search-params) · [Loader dependencies](https://tanstack.com/router/latest/docs/framework/react/guide/data-loading#using-loaderdeps-to-access-search-params)
