# Elements as props and children

**Priority:** MEDIUM

## When

A wrapper needs local state (scroll position, resize, hover) but should not update costly content on each change.

## Rule

Pass already-created JSX as `children` or a named element prop from an owner _outside_ the updating wrapper. The wrapper can update itself without recreating the element for unchanged content. This is composition, not a blanket guarantee against all renders: changes in the element's owner, props, context, or internal state still matter.

**Avoid:** defining or creating the expensive child inside the wrapper's render on every scroll update.

**Prefer:**

```tsx
function ScrollingFrame({ children }: { children: React.ReactNode }) {
  const [top, setTop] = useState(0);
  return (
    <div onScroll={(e) => setTop(e.currentTarget.scrollTop)}>
      <ScrollMarker top={top} />
      {children}
    </div>
  );
}

function Report() {
  return (
    <ScrollingFrame>
      <ExpensiveReport />
    </ScrollingFrame>
  );
}
```

If the wrapper needs to _supply dynamic props_ to content, an element alone is not enough; consider a render prop or split the stateful and expensive parts. Do not use `cloneElement` as a default configuration mechanism.

[React: passing JSX as children](https://react.dev/learn/passing-props-to-a-component#passing-jsx-as-children) · [React: `memo`](https://react.dev/reference/react/memo)
