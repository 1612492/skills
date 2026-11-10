# Security boundaries

**Priority:** CRITICAL

## When

Reading or writing user/tenant data, authenticating requests, or changing Start's CSRF middleware.

## Rule

Every private server function or server route must authorize the caller and check access to the selected resource. A Router `beforeLoad` guard improves navigation UX but cannot protect a callable endpoint. Treat IDs, search params, and client-sent middleware context as untrusted even after shape validation. Keep personalized HTML and RPC responses out of shared public caches; clear account-specific browser query caches on logout/account switch. Use safe error messages.

Start installs CSRF protection for server functions when there is no custom `src/start.ts`. If adding that file, explicitly install `createCsrfMiddleware` for server function requests (or provide an equivalent protection). Do not disable checks merely to make a deployment pass.

**Prefer in a custom `src/start.ts`:**

```tsx
const csrf = createCsrfMiddleware({
  filter: (context) => context.handlerType === "serverFn",
});

export const startInstance = createStart(() => ({
  requestMiddleware: [csrf],
}));
```

Also validate the application's public origin/proxy configuration in deployment. CSRF protection and authentication solve different problems; use both where relevant.

[Server-function security](https://tanstack.com/start/latest/docs/framework/react/guide/server-functions#same-origin-requests) · [Authentication primitives](https://tanstack.com/start/latest/docs/framework/react/guide/authentication-server-primitives)
