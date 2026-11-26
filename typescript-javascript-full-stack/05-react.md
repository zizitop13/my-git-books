# React frontend

## State snapshots
Three calls to setCount(count + 1) in one handler request the same value. To increment three times:
```tsx
setCount(c => c + 1);
setCount(c => c + 1);
setCount(c => c + 1);
```
React batches updates and applies updater functions in order.

## Effects and races
Request A for user 1 starts first; B for user 2 finishes first; A finishes last and overwrites B. Cancel or ignore obsolete work:
```tsx
useEffect(() => {
  const controller = new AbortController();
  async function load() {
    try {
      const r = await fetch("/api/users/" + userId,
        { signal: controller.signal });
      if (!r.ok) throw new Error(String(r.status));
      const data = await r.json();
      if (!controller.signal.aborted) setUser(data);
    } catch (e) {
      if (!controller.signal.aborted) setError(e);
    }
  }
  void load();
  return () => controller.abort();
}, [userId]);
```
Also manage loading state and validate data. Cleanup runs before changed dependencies trigger a new setup and on unmount. Strict Mode may run an extra setup/cleanup cycle in development.

## Memoization
React.memo compares props using Object.is by default. A newly created callback changes identity every render; useCallback can stabilize it. useMemo caches a computed value. Context and local state still cause renders. Measure before optimizing.

## Lists
Use stable keys rather than indices for reorderable items. Keep state immutable, avoid redundant derived state, virtualize very long lists and use a query cache for server state.
