# Question 3 — Promise.all and error handling

**TypeScript / Backend — Medium–Hard**

```typescript
async function fetchUser() {
  await new Promise(r => setTimeout(r, 100));
  return { id: 1, name: "Alice" };
}

async function fetchOrders() {
  await new Promise(r => setTimeout(r, 200));
  throw new Error("Database unavailable");
}

async function fetchRecommendations() {
  await new Promise(r => setTimeout(r, 500));
  console.log("Recommendations loaded");
  return ["Product A", "Product B"];
}

async function loadDashboard() {
  try {
    const [user, orders, recommendations] =
      await Promise.all([
        fetchUser(),
        fetchOrders(),
        fetchRecommendations()
      ]);

    return { user, orders, recommendations };
  } catch (error) {
    console.log("Dashboard failed");
  }
}

loadDashboard();
```

1. What will be printed to the console, and approximately when?
2. What happens to fetchRecommendations() when fetchOrders() fails?
3. How would you modify the code to return the user and recommendations even if orders fail?

<!-- pagebreak -->

## Your answer

1. nothing?

## Feedback

Not quite. Two messages will be printed:

```text
~200 ms: Dashboard failed
~500 ms: Recommendations loaded
```

At 100 ms fetchUser resolves; nothing is printed. At 200 ms fetchOrders rejects, Promise.all immediately rejects, and the catch block prints Dashboard failed. At 500 ms fetchRecommendations finishes.

**Promise.all() does not cancel the remaining promises when one rejects.** This differs from Java's StructuredTaskScope.ShutdownOnFailure, which attempts to interrupt unfinished subtasks when one fails.

<!-- pagebreak -->

## Question 3 — Follow-up quiz

Suppose we want the dashboard to display whatever data is available, even if some services fail. Which approach would you choose?

- [ ] A. Replace Promise.all() with Promise.race()
- [ ] B. Replace Promise.all() with Promise.allSettled()
- [ ] C. Add another try/catch around Promise.all()
- [ ] D. Add await before each individual function call

<!-- pagebreak -->

## Your answer

My answer is D. Please evaluate it and show how to implement partial dashboard results in TypeScript.

## Feedback

D is not the best answer. **The correct answer is B — Promise.allSettled().** D could work with individual try/catch blocks, but sequential awaits make independent requests run one after another.

| Behavior | Promise.all() | Promise.allSettled() |
|---|---|---|
| Coordinates independent concurrent operations | Yes | Yes |
| Rejects when one fails | Yes | No |
| Waits for every operation to settle | Not necessarily | Yes |
| Supports partial results | Requires extra handling | Yes |

```typescript
async function loadDashboard() {
  const [user, orders, recommendations] =
    await Promise.allSettled([
      fetchUser(),
      fetchOrders(),
      fetchRecommendations()
    ]);

  return {
    user: user.status === "fulfilled"
      ? user.value
      : null,

    orders: orders.status === "fulfilled"
      ? orders.value
      : [],

    recommendations:
      recommendations.status === "fulfilled"
        ? recommendations.value
        : []
  };
}
```

After approximately 500 ms:

```json
{
  "user": { "id": 1, "name": "Alice" },
  "orders": [],
  "recommendations": ["Product A", "Product B"]
}
```

Returning [] for failed orders makes failure indistinguishable from a successful empty result. In production, preserve error status as well.

### What about your answer D?

```typescript
async function loadDashboard() {
  let user = null;
  let orders = [];
  let recommendations = [];

  try {
    user = await fetchUser();
  } catch {}

  try {
    orders = await fetchOrders();
  } catch {}

  try {
    recommendations = await fetchRecommendations();
  } catch {}

  return { user, orders, recommendations };
}
```

This takes approximately 800 ms rather than 500 ms because each request starts after the previous one finishes.

**Technical correction:** the sequential example illustrates timing but needs explicit variable types under strict TypeScript settings; the original fetchOrders always throws, so its inferred return type is Promise<never>. The combinators coordinate already-started promises rather than starting operations themselves.
