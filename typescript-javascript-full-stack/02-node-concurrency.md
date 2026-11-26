# Question 2 — Node.js concurrency

**Backend / Node.js — Senior**

Consider this API endpoint:

```typescript
app.get("/calculate", async (req, res) => {
  const result = heavyCalculation();
  res.json({ result });
});

function heavyCalculation(): number {
  let result = 0;

  for (let i = 0; i < 1_000_000_000; i++) {
    result += Math.sqrt(i);
  }

  return result;
}
```

Imagine 100 users send requests simultaneously.

1. What happens to the Node.js server?
2. Does declaring the handler async help?
3. How would you solve this problem in production?
4. Would Promise.all() help here?

<!-- pagebreak -->

## Your answer

As I understand havycalc is a CPU heavy, and asynchronous event loop will sequentially handle each heavy tasks, so requests could stack.

CPU bound tasks should go through multiple cores, but I don't know how to do that with nodes

## Feedback

Your reasoning is correct. **You've identified the main architectural problem: CPU-bound work blocks the Node.js event loop.** I'd rate this answer **7/10**.

While heavyCalculation() executes, other JavaScript request handlers cannot run, timers and Promise callbacks are delayed, and even lightweight endpoints such as /health may become unresponsive.

The async keyword doesn't move work to another thread.

```typescript
async function calculate() {
  return heavyCalculation(); // Still blocks!
}
```

Even this doesn't help:

```typescript
await Promise.resolve().then(heavyCalculation);
```

The calculation executes in a microtask on the same JavaScript thread.

### How to solve it

Node.js provides Worker Threads for CPU-intensive calculations.

```mermaid
flowchart TD
  A[Main event loop] --> B[Worker 1]
  A --> C[Worker 2]
  A --> D[Worker 3]
```

```typescript
import { Worker } from "node:worker_threads";

function calculate(): Promise<number> {
  return new Promise((resolve, reject) => {
    const worker = new Worker("./worker.js");

    worker.once("message", resolve);
    worker.once("error", reject);
    worker.once("exit", code => {
      if (code !== 0) {
        reject(new Error(`Worker exited: ${code}`));
      }
    });
  });
}
```

In production, use a bounded worker pool rather than create a worker per request. Piscina can manage worker reuse and concurrency. Other approaches include separate processing services, job queues, and horizontal scaling.

Promise.all() coordinates asynchronous operations but doesn't make synchronous CPU work execute in parallel. It is useful for concurrent I/O.

| Java | Node.js |
|---|---|
| Platform threads | Worker threads |
| Virtual threads | No direct equivalent |
| ExecutorService | Worker pool |
| CompletableFuture.allOf() | Promise.all() |
| CPU parallelism via threads | CPU parallelism via workers |

Java virtual threads don't make CPU-intensive calculations faster either. Their primary benefit is efficient concurrency for blocking operations.
