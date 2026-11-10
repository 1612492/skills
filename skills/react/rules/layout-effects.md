# Layout effects

**Priority:** MEDIUM

## When

DOM measurement needs to change the same frame before paint, and a normal Effect visibly flickers.

## Rule

Use `useLayoutEffect` only for a real pre-paint read/write after React commits (for example, measuring a tooltip then repositioning it). It blocks painting, so keep the work small. Prefer CSS layout and declarative positioning first. `useEffect` is usually appropriate for synchronization that need not block paint. Effects and layout effects do not run during server rendering; don't assume SSR markup can contain a browser-only measurement.

**Avoid:** replacing every `useEffect` with `useLayoutEffect` to hide a slow render or an unnecessary state update.

**Prefer, if measurement is necessary:**

```tsx
function MeasuredLabel() {
  const ref = useRef<HTMLSpanElement>(null);
  const [height, setHeight] = useState(0);
  useLayoutEffect(() => {
    setHeight(ref.current?.getBoundingClientRect().height ?? 0);
  }, []);
  return (
    <>
      <span ref={ref}>Label</span>
      <span>{height}px</span>
    </>
  );
}
```

This measures on mount only; for changing content or responsive layouts, use the appropriate remeasurement mechanism (often `ResizeObserver`) and clean it up. SSR may initially render with a placeholder height; choose a CSS solution or client-only rendering if that causes unacceptable mismatch/flicker.

[React: `useLayoutEffect`](https://react.dev/reference/react/useLayoutEffect)
