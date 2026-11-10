# Memoization with evidence

**Priority:** MEDIUM

## When

Profiling shows repeated costly work on a hot interaction, or a stable value/function is required by a memoized child or Effect dependency.

## Rule

First localize state and remove unnecessary Effects. `memo` can skip parent-driven renders with equal props but does not block the component's own state or context updates. `useMemo` caches a _pure calculation_; `useCallback` caches a _function identity_. Dependencies and props compare using `Object.is`, so newly allocated objects/functions break shallow comparisons. React Compiler, when enabled, often provides the equivalent caching automatically; check the project before adding manual memoization.

**Avoid:** wrapping every handler in `useCallback` or every value in `useMemo` without a consumer that benefits.

**Prefer, only after measuring:**

```tsx
const Results = memo(function Results({ items }: { items: Item[] }) {
  return <ResultTable items={items} />;
});

function Search({ items, term }: { items: Item[]; term: string }) {
  const visible = useMemo(() => expensiveFilter(items, term), [items, term]);
  return <Results items={visible} />;
}
```

If `items` is recreated on every parent render or filtering is cheap, reconsider this design. Never omit dependencies to fake stability; compare real before/after timings in a production build. Custom `memo` comparators must include function props to avoid stale closures.

[React: `memo`](https://react.dev/reference/react/memo) · [React Compiler](https://react.dev/learn/react-compiler)
