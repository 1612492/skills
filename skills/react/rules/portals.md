# Portals and CSS containment

**Priority:** MEDIUM

## When

A menu, tooltip, or dialog is clipped by `overflow`, obscured by stacking contexts, or affected by transformed ancestors.

## Rule

Inspect CSS positioning and stacking before changing React structure. A portal moves the DOM subtree to another container while preserving its React parent relationships: context still flows and React events still bubble through the React tree. It does not solve anchoring, keyboard behavior, focus management, or event propagation automatically. For dialogs prefer a proven accessible dialog or native `<dialog>` where suitable.

**Avoid:** adding an enormous `z-index` to an overlay trapped inside an ancestor stacking context.

**Prefer, when the DOM container must be moved:**

```tsx
function Overlay({ children }: { children: React.ReactNode }) {
  const host = typeof document === 'undefined' ? null : document.getElementById('overlay-root');
  return host ? createPortal(children, host) : null;
}
```

Ensure the host exists before first render or mount it safely; design SSR/hydration behavior for the framework. When dismissing on click, remember that portal events bubble through the *React* tree; stop propagation or reposition the portal in the React tree as appropriate. For positioning, account for scroll and viewport changes rather than assuming a portal fixes coordinates.

[React: `createPortal`](https://react.dev/reference/react-dom/createPortal)
