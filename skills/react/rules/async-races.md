# Async races

**Priority:** HIGH

## When

An older request overwrites results for a newer search, route, or selected entity; updates arrive after unmount.

## Rule

Request completion order is not request start order. Prefer the existing framework/query library's race-handling policy. For manual Effect-based fetches, clean up on dependency change/unmount and prevent stale results (including failures) from updating the current UI. `AbortController` can reduce wasted work when supported; still guard against non-abortable or already-completed async steps. A key can intentionally reset a subtree but is not a general replacement for request ownership.

**Avoid:**

```tsx
useEffect(() => {
  fetchResults(term).then(setResults);
}, [term]);
```

**Prefer (if manual fetching is appropriate):**

```tsx
useEffect(() => {
  let ignore = false;
  const controller = new AbortController();
  setStatus('loading');
  fetchResults(term, { signal: controller.signal })
    .then(data => { if (!ignore) { setResults(data); setStatus('success'); } })
    .catch(() => {
      if (!ignore && !controller.signal.aborted) setStatus('error');
    });
  return () => { ignore = true; controller.abort(); };
}, [term]);
```

Adapt `fetchResults` and state types to the project; never swallow non-abort errors silently. Test delayed responses arriving out of order and component unmount. Aborting a request does not retroactively undo server-side effects.

[React: fetching with Effects](https://react.dev/learn/you-might-not-need-an-effect#fetching-data)
