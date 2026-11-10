# Closures and timers

**Priority:** HIGH

## When

A delayed handler sees old props/state, debounce never fires, or listeners use stale callbacks.

## Rule

Every render creates its own snapshot of values. Keep dependency arrays honest; use functional state updates when next state depends on previous state. A debounced/throttled function needs a stable timer across relevant renders, but its callback must see the latest values when required. React 19.2+ has `useEffectEvent` for non-reactive logic _inside Effects_; it is not a general-purpose event handler or a shortcut for missing dependencies. For general timers, use a maintained utility or an encapsulated hook rather than hand-rolling a complicated throttle.

**Avoid:** creating `debounce(send, 300)` in render, then invoking it on every keystroke; each render starts with a fresh timer.

**Prefer (debouncing a value for an Effect):**

```tsx
function SearchBox() {
  const [query, setQuery] = useState("");
  const [debounced, setDebounced] = useState("");
  useEffect(() => {
    const id = setTimeout(() => setDebounced(query), 300);
    return () => clearTimeout(id);
  }, [query]);
  // A framework loader/query hook can consume `debounced` with its own race handling.
  return (
    <>
      <input value={query} onChange={(e) => setQuery(e.target.value)} />
      <span>Settled query: {debounced}</span>
    </>
  );
}
```

The example only delays the value; it does not fetch or solve response races. For a stable debounced callback, ensure both timer cleanup on unmount and access to the latest callback (through a suitable hook/ref or a library). Test rapid input, changed props, and unmount.

[React: state snapshots](https://react.dev/learn/state-as-a-snapshot) · [React: removing Effect dependencies](https://react.dev/learn/removing-effect-dependencies)
