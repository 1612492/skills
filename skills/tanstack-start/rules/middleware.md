# Middleware

**Priority:** HIGH

## When

Reusing request logging, server-side sessions, authorization, or shared server-function logic.

## Rule

Request middleware applies to SSR, server routes, and server functions; function middleware applies to server functions and can also have client hooks. Call `next()` to continue the chain and use its `context` for trusted server-derived values. Client-sent context is untrusted and is not sent automatically. Attach authorization middleware to **each private endpoint** (or a deliberately comprehensive global policy); route guards alone are not sufficient.

**Prefer:**

```tsx
const requireSession = createMiddleware().server(async ({ next, request }) => {
  const session = await readSession(request.headers);
  if (!session) throw new Error("Unauthorized");
  return next({ context: { session } });
});

export const getMyProfile = createServerFn()
  .middleware([requireSession])
  .handler(({ context }) => loadProfile(context.session.userId));
```

Here `readSession` derives identity from the server-verified request, not a client-provided user ID. Check middleware execution order and response headers when installing a global `src/start.ts` file; preserve CSRF protection there.

[Middleware](https://tanstack.com/start/latest/docs/framework/react/guide/middleware)
