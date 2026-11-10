# State locality and render boundaries

**Priority:** HIGH

## When

A local interaction (dialog, hover, scroll, input) causes unrelated expensive UI to re-render.

## Rule

The component calling `useState` or a stateful custom hook is the component that updates. Its descendants may render as a result; merely moving the same hook logic into a custom hook does not move the render boundary. Move state into the smallest component that owns the interaction, unless other components genuinely need to share it. A prop changing by itself does not schedule a render; an update must occur first. Context and external subscriptions can update consumers independently of their ancestors.

**Avoid:** keeping a dialog's local `open` state in an expensive page component just because the button is on that page.

**Prefer:**

```tsx
function HelpDialogButton() {
  const [open, setOpen] = useState(false);
  return <>
    <button onClick={() => setOpen(true)}>Help</button>
    {open && <HelpDialog onClose={() => setOpen(false)} />}
  </>;
}

function Dashboard() {
  return <><HelpDialogButton /><LargeChart /></>;
}
```

The dialog toggle does not update `Dashboard`; other updates may still render `LargeChart`. If state must be shared, lift it only to the nearest common owner or use the project's existing state solution. Profile before adding optimization.

[React: state structure](https://react.dev/learn/choosing-the-state-structure) · [React: `memo`](https://react.dev/reference/react/memo)
