# Fetching and waterfalls

**Priority:** HIGH

## When

Initial page content arrives slowly, a child fetch waits for a parent, or loading behavior feels sequential.

## Rule

Distinguish initial/route data from interaction-triggered requests. Inspect the network waterfall before optimizing render counts. If requests are independent, start them together at the route/framework loading boundary; if one truly depends on the other, keep the dependency explicit. Prefer framework loaders/server fetching or the project's query cache for preloading, deduplication, retries, and hydration. An Effect fetch starts after the component commits and won't populate initial server HTML. Suspense requires a Suspense-enabled data source; wrapping arbitrary Effect-based `fetch` with `<Suspense>` does not make it suspend.

**Avoid:** mounting a parent, fetching its data in an Effect, and *then* mounting a child to fetch independent data.

**Prefer, at a suitable loader/server boundary:**

```ts
async function loadPage() {
  const [profile, suggestions] = await Promise.all([
    fetchProfile(),
    fetchSuggestions(),
  ]);
  return { profile, suggestions };
}
```

Handle partial failure according to product requirements; use `Promise.allSettled` or separate boundaries if one failure should not hide all content. Don't bypass an established cache by adding a second ad hoc request.

[React: data fetching alternatives](https://react.dev/learn/synchronizing-with-effects#what-are-good-alternatives-to-data-fetching-in-effects)
