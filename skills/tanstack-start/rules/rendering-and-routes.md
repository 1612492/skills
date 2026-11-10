# Rendering and server routes

**Priority:** HIGH

## When

Implementing an external HTTP API, debugging SSR/hydration, or separating client/server code.

## Rule

Use `server.handlers` on a file route when an external caller needs raw HTTP semantics; use a server function for internal typed RPC. Load critical route data before rendering when SEO/first paint needs it. Keep per-request data isolated on the server and return only serializable, authorized data. Direct navigation, client transitions, hydration, and streaming can take different paths: test them all. Mark non-public content `private` or `no-store` as appropriate across the final HTML and endpoint responses.

**Prefer for an external endpoint:**

```tsx
export const Route = createFileRoute("/api/health")({
  server: {
    handlers: {
      GET: () => Response.json({ status: "ok" }),
    },
  },
});
```

This public response contains no user data. For private routes, verify session and resource authorization inside the handler; validate the body of writes. Keep `.server.ts` helpers server-only and client-safe shared schemas in regular modules. Avoid module-scope mutable request state.

[Server routes](https://tanstack.com/start/latest/docs/framework/react/guide/server-routes) · [Start overview](https://tanstack.com/start/latest/docs/framework/react/overview)
