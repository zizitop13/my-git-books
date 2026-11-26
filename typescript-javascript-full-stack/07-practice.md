# Mock interview

## Questions
1. Why does async not fix a CPU-heavy handler?
2. What happens to siblings when Promise.all rejects?
3. Why can a fresh object literal fail when a compatible variable passes?
4. How do you validate external JSON?
5. Why do three setCount(count + 1) calls increment once?
6. How can an old request overwrite new React state?
7. Why does memo fail with a new callback?
8. How do you prevent duplicate payments after a timeout?
9. How do you bound AI response streaming?
10. How do you test concurrent inventory updates?

<!-- pagebreak -->

## Answers
1. CPU work blocks the event-loop thread; offload to bounded workers.
2. They continue unless explicitly cancelled.
3. Structural typing plus excess property checking.
4. Check HTTP status and parse unknown with a runtime schema.
5. One render snapshot; use functional updates.
6. Out-of-order responses; abort or ignore obsolete effects.
7. New reference identity; stabilize only when useful.
8. Persist an idempotency key atomically with the outcome.
9. Enforce concurrency and token budgets, cancellation and backpressure.
10. Run overlapping real database transactions and assert the invariant.

## References
- [TypeScript Handbook](https://www.typescriptlang.org/docs/handbook/intro.html)
- [MDN JavaScript Guide](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide)
- [Node documentation](https://nodejs.org/docs/latest/api/)
- [React documentation](https://react.dev/)
- [Zod](https://zod.dev/)

These are independent practice questions, not verified Alignerr assessment content.
