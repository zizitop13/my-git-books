# Event loop and promises

Synchronous code runs to completion. Promise reactions and queueMicrotask callbacks run before subsequent timers. Node also has nextTick and event-loop phases.

```ts
console.log("A");
setTimeout(() => console.log("B"), 0);
Promise.resolve().then(() => console.log("C"));
queueMicrotask(() => console.log("D"));
async function test() {
  console.log("E");
  await Promise.resolve();
  console.log("F");
}
test();
console.log("G");
```
Predict the output.

<!-- pagebreak -->

Answer: A E G C D F B. Async functions start synchronously until suspension; await does not create a thread.

## Promise combinators
| Method | Behavior |
| --- | --- |
| all | All fulfill or first rejection |
| allSettled | Wait for every outcome |
| race | First settlement |
| any | First fulfillment, or AggregateError |

Promise.all does not cancel siblings. For independent user, orders and recommendations requests taking 100, 200 (failure), and 500 ms, all rejects around 200 ms, but recommendations still finish at 500 ms. Use allSettled for partial results and preserve error status. Sequential awaits with individual error handling take around 800 ms.

```ts
const results = await Promise.allSettled([
  fetchUser(), fetchOrders(), fetchRecommendations()
]);
```
Use AbortSignal for cooperative cancellation and a bounded concurrency pool for large batches.
