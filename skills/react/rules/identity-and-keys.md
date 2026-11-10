# Identity and keys

**Priority:** HIGH

## When

Component state disappears unexpectedly, remains when a new entity is shown, or follows the wrong row after list reorder.

## Rule

React associates state with component type and position in the tree, with keys distinguishing siblings. Define component types outside other components. Use stable entity IDs as keys for dynamic lists; an array index is unsafe when items insert, remove, or reorder. Change a key deliberately when the *whole subtree* should reset; changing it also remounts DOM and Effects. Keys are scoped to siblings, not globally unique.

**Avoid:**

```tsx
{rows.map((row, index) => <EditableRow key={index} row={row} />)}
```

**Prefer:**

```tsx
{rows.map(row => <EditableRow key={row.id} row={row} />)}

// Reset a form when switching to a different entity:
<EditForm key={selectedId} recordId={selectedId} />
```

Use a key outside a list only when you intentionally control identity. Prefer derived state over using remounts to patch unrelated bugs. Changing the owner or type can reset state even when a child has a key.

[React: preserving and resetting state](https://react.dev/learn/preserving-and-resetting-state) · [React: lists and keys](https://react.dev/learn/rendering-lists#keeping-list-items-in-order-with-key)
