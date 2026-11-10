# Render props and composition

**Priority:** MEDIUM

## When

A component should let its caller control an icon, action, or subtree while providing that UI with its own state or computed props.

## Rule

Use a React element prop when the wrapper does not need to inject changing values. Use a render function when it _does_ need to supply values. A render prop is called while the owner renders; it is not a free performance optimization. Prefer ordinary hooks when stateful logic is independent of the rendered DOM; render props remain useful when the state is tied to a wrapper-owned element.

**Avoid:** passing `<StatusLabel />` and then attempting to mutate its props after creation.

**Prefer:**

```tsx
function ToggleArea({ renderStatus }: { renderStatus: (active: boolean) => React.ReactNode }) {
  const [active, setActive] = useState(false);
  return (
    <div>
      <button onClick={() => setActive((value) => !value)}>Toggle</button>
      {renderStatus(active)}
    </div>
  );
}

<ToggleArea renderStatus={(active) => <StatusLabel active={active} />} />;
```

Define reusable component types at module scope; do not define a nested component type in a render function and then render it as JSX if its state should persist.

[React: passing props](https://react.dev/learn/passing-props-to-a-component) · [React: preserving state](https://react.dev/learn/preserving-and-resetting-state)
