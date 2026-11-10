# Refs and imperative handles

**Priority:** HIGH

## When

Accessing a DOM node, storing a mutable timer/handle, or exposing an imperative focus/scroll API.

## Rule

Updating a ref is synchronous and does not schedule a render. Do not use a ref for values whose changes should update the screen, and do not read/write `ref.current` during render except predictable initialization. Use DOM refs in event handlers or Effects; expose the smallest imperative surface needed. Prefer props for declarative behavior such as `open`.

**Avoid:** setting `ref.current` in an event and expecting JSX to reflect the new value.

**Prefer (React 19+):**

```tsx
function SearchField({ ref }: { ref: React.Ref<HTMLInputElement> }) {
  return <input ref={ref} type="search" />;
}

function Toolbar() {
  const inputRef = useRef<HTMLInputElement>(null);
  return (
    <>
      <SearchField ref={inputRef} />
      <button onClick={() => inputRef.current?.focus()}>Focus search</button>
    </>
  );
}
```

For React 18 or earlier, use `forwardRef` when forwarding the special `ref` prop through a function component. A normal, differently named prop carrying a ref object is another option when appropriate. Use `useImperativeHandle` only for a constrained custom API.

[React: `useRef`](https://react.dev/reference/react/useRef) · [React: `forwardRef`](https://react.dev/reference/react/forwardRef) · [React: `useImperativeHandle`](https://react.dev/reference/react/useImperativeHandle)
