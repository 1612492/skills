---
name: feature-sliced-design
description: Structure or review Next.js and TanStack Start applications with Feature-Sliced Design (FSD). Use when deciding where routes, pages, widgets, features, entities, shared code, and app-wide setup belong, or when checking slice public APIs and import boundaries. Adapt to the existing framework and avoid unnecessary migrations.
---

# Feature-Sliced Design for Next.js and TanStack Start

Use FSD to clarify ownership, not to produce every possible folder. Framework route files remain where the framework expects them; FSD slices own the domain-specific implementation behind those routes. Apply to Next.js App Router or Pages Router and TanStack Start with TanStack Router; inspect the target project's versions and conventions first.

## Workflow

1. Inspect the target project's router, source root, aliases, runtime boundaries, and existing feature organization. Identify the concrete structural problem before proposing a change.
2. Decide the smallest owner for the code: a page for route-specific behavior, a widget for a substantial reusable UI block, a feature for a meaningful reusable user interaction, an entity for a business concept, `shared` for domain-agnostic primitives, or `app` for app-wide composition. Do not create unused layers or split one-off page code prematurely.
3. Keep route definitions and framework-required exports in their expected locations. Delegate page UI and reusable behavior to slices without hiding route-level loading, metadata, errors, authentication, or server handlers that the framework must discover.
4. Enforce downward dependencies and narrow slice public APIs. Check server/client imports, direct navigation, and build output after moving code.

| Task                                                    | Read                                                      |
| ------------------------------------------------------- | --------------------------------------------------------- |
| Choosing layers, slices, segments, or import boundaries | [Layers and public APIs](rules/layers-and-public-apis.md) |
| Next.js App Router or Pages Router structure            | [Next.js integration](rules/nextjs.md)                    |
| TanStack Start routes and server boundaries             | [TanStack Start integration](rules/tanstack-start.md)     |

For framework behavior, also use `react`, `tanstack-router`, `tanstack-start`, or `tanstack-integration` as applicable. Base architecture choices on the [FSD layer reference](https://feature-sliced.design/docs/reference/layers) and the target application's needs, not a mandatory scaffold.
