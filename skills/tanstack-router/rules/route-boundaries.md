# Route boundaries

**Priority:** HIGH

## When

Protecting a page's UX, returning 404, or recovering from a loader error.

## Rule

Use `beforeLoad` for route-level redirects/guards and route `errorComponent`/`notFoundComponent` for failures. Guarded UI is _not_ authorization: every private server function or API must enforce permissions independently. For failed loaders, `router.invalidate()` coordinates reload and error reset; a boundary's `reset()` alone mainly retries rendering. Narrow `error: unknown` before reading properties.

**Prefer:**

```tsx
export const Route = createFileRoute("/account")({
  beforeLoad: ({ context, location }) => {
    if (!context.auth.isSignedIn) {
      throw redirect({ to: "/login", search: { redirect: location.href } });
    }
  },
  loader: () => getAccount(),
  errorComponent: AccountError,
});

function AccountError({ error }: { error: unknown }) {
  const router = useRouter();
  return (
    <div>
      {error instanceof Error ? error.message : "Unable to load account"}
      <button onClick={() => router.invalidate()}>Retry</button>
    </div>
  );
}
```

The root route must type/provide `auth` in context for this example. Avoid displaying private error details to end users; test unauthenticated direct visits and expired sessions.

[Authenticated routes](https://tanstack.com/router/latest/docs/framework/react/guide/authenticated-routes) · [Error handling](https://tanstack.com/router/latest/docs/framework/react/guide/data-loading#handling-errors)
