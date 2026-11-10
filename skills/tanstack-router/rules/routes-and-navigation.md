# Typed routes and navigation

**Priority:** HIGH

## When

Adding a nested page, path parameter, layout, or link.

## Rule

Follow the project's file-based or code-based route convention; keep generated route tree output under its generator's control. In file-based routing, route paths in `createFileRoute` must agree with the route file convention. Register the router's type for typed `Link`/navigation and prefer `Link` for actual navigation (including opening in a new tab).

**Prefer:**

```tsx
// In a file-based route for /products/$productId:
export const Route = createFileRoute("/products/$productId")({
  component: ProductPage,
});

function ProductPage() {
  const { productId } = Route.useParams();
  return <ProductView productId={productId} />;
}

<Link to="/products/$productId" params={{ productId: product.id }}>
  View product
</Link>;
```

The snippet assumes `product` is in scope and the project's route tree is generated/registered. Avoid string-interpolated pathnames that discard route typing. Use pathless layouts for shared UI/guards without adding a URL segment; test index and nested matches after renaming files.

[File-based routing](https://tanstack.com/router/latest/docs/framework/react/routing/file-based-routing) · [Type safety](https://tanstack.com/router/latest/docs/framework/react/guide/type-safety)
