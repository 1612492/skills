---
name: tanstack-start
description: Build secure TanStack Start React applications with validated server functions, per-request authorization, middleware, CSRF protection, server routes, and SSR-aware code. Use when working with @tanstack/react-start, createServerFn, src/start.ts, server handlers, or Start deployment. Use tanstack-integration when adding Query to Start.
---

# TanStack Start (React)

Check the installed Start version and bundler before proposing APIs; the [current Start documentation](https://tanstack.com/start/latest/docs/framework/react/overview) describes a stable full-stack framework. Do not assume experimental React Server Components are enabled. Route conventions come from TanStack Router.

## Workflow

1. Determine whether an operation runs on server, client, or both. Classify it as a server function (app-internal RPC), server route (external HTTP), or route loader (navigation coordination).
2. Validate untrusted inputs and enforce authentication _inside every private endpoint_. Keep secrets and server-only dependencies out of client-facing code.
3. Read relevant rules. Verify direct endpoint access, SSR and hydrated navigation, invalid inputs, and production deployment behavior.

| Task                                    | Rule                                                         |
| --------------------------------------- | ------------------------------------------------------------ |
| Internal read/write RPC                 | [Server functions](rules/server-functions.md)                |
| Private resources, auth, CSRF           | [Security boundaries](rules/security.md)                     |
| Shared request behavior                 | [Middleware](rules/middleware.md)                            |
| SSR, public HTTP endpoints, environment | [Rendering and server routes](rules/rendering-and-routes.md) |

For shared cached data between Start loaders and components, use `tanstack-integration`. Avoid putting database credentials or service keys into loader responses, serialized query data, or browser-visible environment variables.
