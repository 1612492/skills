# Infinite queries

**Priority:** MEDIUM

## When

Building cursor pagination, load-more lists, or infinite scroll.

## Rule

In Query v5, supply `initialPageParam` and `getNextPageParam`; include filters in the key, but not the current cursor (the hook manages page params). Guard `fetchNextPage` against missing next pages and overlapping fetches. An infinite query shares one cache entry for all pages; a background refetch can conflict with a simultaneous page fetch. `maxPages` can bound memory and sequential refetch cost.

**Prefer:**

```tsx
const feed = useInfiniteQuery({
  queryKey: ['feed', filter],
  initialPageParam: null as string | null,
  queryFn: ({ pageParam }) => fetchFeed({ filter, cursor: pageParam }),
  getNextPageParam: last => last.nextCursor ?? undefined,
})

<button
  disabled={!feed.hasNextPage || feed.isFetching}
  onClick={() => feed.fetchNextPage()}
>Load more</button>
```

Render `feed.data.pages` with stable item IDs; handle initial loading and errors separately from `isFetchingNextPage`. When manually updating infinite data, preserve both `pages` and `pageParams`.

[Infinite queries](https://tanstack.com/query/latest/docs/framework/react/guides/infinite-queries)
