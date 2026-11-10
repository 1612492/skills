# Layers and public APIs

**Priority:** HIGH

## When

Choosing the home for new code, reviewing imports, or incrementally adopting FSD in an existing app.

## Rule

Use the smallest useful set of layers, ordered `app → pages → widgets → features → entities → shared`. In Next.js, name the FSD `app` and `pages` layers `_app` and `_pages` to avoid framework routing conflicts. `processes` is deprecated; don't introduce it. A slice may import other slices only from strictly lower layers; siblings on the same layer cannot import each other. `app` and `shared` are exceptions: they contain segments rather than business slices, and imports among their own segments are allowed.

- `pages`: route-specific screen, data composition, loading/error UI. Keep non-reused behavior here.
- `widgets`: substantial self-contained UI blocks reused across pages or independent parts of a large page.
- `features`: reusable user interactions (for example, submit a review), not every button or hook.
- `entities`: product-domain concepts and their presentation/data contracts; compose multiple entities higher up.
- `shared`: domain-agnostic UI, focused libraries, API foundations, and config; avoid a generic dumping ground.
- `app`: global providers, styles, router/entry configuration, and app-wide integration.

Within a slice, use purpose-oriented segments such as `ui`, `model`, `api`, `lib`, and `config` only when needed. Use an explicit slice-level `index.ts` public API for imports from other slices; avoid `export *` and deep imports into another slice. Within a slice, import directly via relative paths, not its own `index.ts` (which can create cycles). For large `shared/ui` or `shared/lib`, expose focused entry points instead of one giant barrel. If two entities must refer to each other's types, prefer composing them higher up; use FSD's explicit `@x` cross-reference API only for a necessary entity-to-entity exception.

**Check:** Look for upward or sibling-slice imports, accidental client exposure of server modules, cycles, and breaking changes to public exports. Migrate one cohesive area at a time; keep existing paths when reorganization adds no value. An architectural linter such as [Steiger](https://github.com/feature-sliced/steiger) can help when the project warrants it.

[FSD layers](https://feature-sliced.design/docs/reference/layers) · [Public API](https://feature-sliced.design/docs/reference/public-api) · [Slices and segments](https://feature-sliced.design/docs/reference/slices-segments)
