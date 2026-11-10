# Context subscriptions

**Priority:** HIGH

## When

A provider update re-renders many consumers, or a `memo`-wrapped context consumer still updates.

## Rule

Consumers of a context update when its value changes; `memo` does not block updates from context consumed inside the memoized component. Keep providers close to users, split independent values where useful, and stabilize object/function values only when they actually cause excess work. For fine-grained subscriptions, use an existing selector-capable store or a suitable external-store pattern rather than expecting plain `useContext` to select one field.

**Avoid:** one context holding fast-changing selection, theme, and unrelated actions, then relying on `memo` around consumers.

**Prefer:**

```tsx
const ThemeContext = createContext<'light' | 'dark'>('light');
const SelectionContext = createContext<string | null>(null);

function Providers({ theme, selectedId, children }: {
  theme: 'light' | 'dark'; selectedId: string | null; children: React.ReactNode;
}) {
  return <ThemeContext.Provider value={theme}>
    <SelectionContext.Provider value={selectedId}>{children}</SelectionContext.Provider>
  </ThemeContext.Provider>;
}
```

Splitting contexts limits *context subscriptions*, not parent-driven renders; inspect both before refactoring. Follow the provider syntax supported by the installed React version.

[React: `useContext`](https://react.dev/reference/react/useContext) · [React: `memo`](https://react.dev/reference/react/memo)
