---
name: react
description: Build, debug, and review React components using advanced composition, rendering and reconciliation, hooks, context, refs, effects, portals, data fetching, and error-handling patterns. Use for React/TSX performance, stale state, remounts, async races, or UI architecture; adapt advice to the project's React version and framework.
---

# React development

Apply the relevant rules below to the actual task, not all of them by default. Favor correct, simple code over speculative optimization.

## Workflow

1. Check the project's React version, framework, data-fetching tools, React Compiler setup, and existing conventions before choosing an API or pattern.
2. Identify the symptom and its cause: state owner and render boundary; component type, position and key; stale closure or Effect; DOM/CSS containment; network dependency; or error source.
3. Read only the matching rule files. Start with correctness; use composition and state locality before manual memoization. Do not add a library or rewrite architecture unless the task justifies it.
4. For performance claims, profile the relevant interaction (prefer production builds) and compare before/after. A re-render alone is not a bug, and a render is not necessarily a DOM update.
5. Verify behavior for changing props, unmount/remount, Strict Mode development checks, and loading/error states where relevant. Run the project's tests, lint, and build when available.

## Rule index

| Symptom or task                                     | Read                                                            |
| --------------------------------------------------- | --------------------------------------------------------------- |
| Slow update; hook state triggers broad renders      | [State locality and render boundaries](rules/state-locality.md) |
| Stateful wrapper; expensive children                | [Elements as props and children](rules/elements-as-props.md)    |
| Child UI needs data or configuration from a wrapper | [Render props and composition](rules/render-props.md)           |
| Repeated expensive renders/calculations             | [Memoization with evidence](rules/memoization.md)               |
| Unexpected state reset/preservation; dynamic lists  | [Identity and keys](rules/identity-and-keys.md)                 |
| Reusable cross-cutting wrapper                      | [Higher-order components](rules/higher-order-components.md)     |
| Context changes update too much UI                  | [Context subscriptions](rules/context.md)                       |
| DOM access or imperative API                        | [Refs and imperative handles](rules/refs.md)                    |
| Stale callback; broken debounce/throttle            | [Closures and timers](rules/closures-and-timers.md)             |
| Flicker or layout measurement                       | [Layout effects](rules/layout-effects.md)                       |
| Overlays clipped or hidden behind content           | [Portals and CSS](rules/portals.md)                             |
| Slow loading due to network sequence                | [Fetching and waterfalls](rules/fetching-waterfalls.md)         |
| Old network result overwrites new data              | [Async races](rules/async-races.md)                             |
| Render, event, or promise error                     | [Error boundaries](rules/error-boundaries.md)                   |

## Version and scope guardrails

- Prefer the installed React version's [official documentation](https://react.dev/reference/react) when APIs or behavior differ. For React 19, ordinary function components can accept `ref` as a prop; older React versions need `forwardRef` for a special `ref` prop. React Compiler, if configured, may eliminate the need for some manual memoization.
- Treat render props and higher-order components as tools for concrete composition problems, not defaults over plain components and hooks. Use [Effects](https://react.dev/learn/you-might-not-need-an-effect) to synchronize with external systems; derive render data during render and handle user actions in event handlers.
- For data fetching, use the project's framework loaders/server fetching or established cache first. Do not imply that Suspense automatically makes an arbitrary `fetch` inside an Effect suspend.
