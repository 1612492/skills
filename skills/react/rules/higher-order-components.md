# Higher-order components

**Priority:** LOW

## When

Integrating an existing API that decorates components with shared behavior (for example, analytics, legacy lifecycle handling, or a library-provided wrapper).

## Rule

A higher-order component (HOC) accepts a component and returns a component. Create it outside render so its type stays stable; forward relevant props and preserve behavior. Prefer a hook for reusable logic or an ordinary wrapper for layout when they solve the problem with less indirection. A HOC is not inherently faster.

**Avoid:** `const Tracked = withTracking(View)` inside a component body; this creates a new type on each render and can remount its subtree.

**Prefer:**

```tsx
function withTracking<P extends object>(View: React.ComponentType<P>) {
  return function Tracked(props: P) {
    usePageTracking();
    return <View {...props} />;
  };
}

const TrackedAccount = withTracking(Account);
```

If the wrapper must accept a `ref`, handle it using the API supported by the project's React version; do not silently swallow the ref. Beware of prop name collisions and unnecessary wrapper depth.

[React: component identity](https://react.dev/learn/preserving-and-resetting-state) · [React: custom hooks](https://react.dev/learn/reusing-logic-with-custom-hooks)
