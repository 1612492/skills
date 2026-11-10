# Error boundaries

**Priority:** HIGH

## When

A render throws, a subtree crashes, or async/event errors need user-visible handling.

## Rule

Place error boundaries around meaningful parts of the UI so one failed area need not take down the rest. A boundary handles errors thrown during rendering by descendants; it does **not** automatically catch errors in event handlers, timers, arbitrary promises, or the boundary itself. Handle those at their origin with `try/catch` or the project's async/query error state. If a library supports surfacing an async failure during render to a boundary, use its documented mechanism; never throw directly from an unhandled promise callback.

**Avoid:** expecting a boundary to catch a failed `await save()` in an `onClick` handler.

**Prefer:**

```tsx
async function handleSave() {
  try {
    await saveDraft();
    setMessage('Saved');
  } catch {
    setMessage('Save failed. Please retry.');
  }
}

<ErrorBoundary fallback={<WidgetFallback />}>
  <RiskyWidget />
</ErrorBoundary>
```

Use the framework's boundary convention or an existing boundary package where present; plain React still needs a class-based boundary implementation or a library. Provide a retry/reset path, decide whether changing a key should reset the failed subtree, and test both render and asynchronous failures.

[React: error boundary guidance](https://react.dev/reference/react/Component#catching-rendering-errors-with-an-error-boundary)
