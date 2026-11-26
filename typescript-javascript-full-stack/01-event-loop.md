# Question 1 — JavaScript Event Loop

**Node.js / JavaScript — Medium**

What will this code print, and in what order?

```typescript
console.log("A");

setTimeout(() => {
  console.log("B");
}, 0);

Promise.resolve().then(() => {
  console.log("C");
});

queueMicrotask(() => {
  console.log("D");
});

async function test() {
  console.log("E");
  await Promise.resolve();
  console.log("F");
}

test();

console.log("G");
```

Explain **why** the output appears in that order, including the difference between the call stack, microtask queue, and timer callbacks.

<!-- pagebreak -->

## Your answer

A then in any order B C D E F then G

## Feedback

Not quite. The order is **deterministic**, not random.

```text
A
E
G
C
D
F
B
```

JavaScript executes synchronous code first, then microtasks, then the timer callback.

| Output | Reason |
|---|---|
| A | Synchronous console.log |
| E | test() is called synchronously; execution continues until await |
| G | Remaining synchronous code |
| C | First queued Promise microtask |
| D | Second queued microtask |
| F | Continuation of test() after await |
| B | setTimeout callback, processed later |

An async function does not automatically execute asynchronously. E prints immediately. Only the continuation after await is scheduled as a microtask. Promise.then() and queueMicrotask() use the same microtask queue, preserving their scheduling order.

### Java comparison

Unlike Java's CompletableFuture, where asynchronous continuations may execute on worker threads, ordinary JavaScript Promise callbacks execute on the JavaScript thread through the microtask queue. Node.js can perform I/O concurrently, but JavaScript callbacks normally run on the event-loop thread.
