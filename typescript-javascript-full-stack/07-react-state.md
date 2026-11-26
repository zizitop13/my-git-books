# Question 7 — React hooks

**React / TypeScript — Medium**

```tsx
import { useState } from "react";

function Counter() {
  const [count, setCount] = useState(0);

  function increment() {
    setCount(count + 1);
    setCount(count + 1);
    setCount(count + 1);
  }

  return (
    <button onClick={increment}>
      Count: {count}
    </button>
  );
}
```

1. When the user clicks the button once, what number will be displayed?

<textarea name="q7-1" rows="3" placeholder="Your answer"></textarea>

2. Why?

<textarea name="q7-2" rows="3" placeholder="Your answer"></textarea>

3. How would you modify increment() so that one click increases the counter by 3?

<textarea name="q7-3" rows="3" placeholder="Your answer"></textarea>


<!-- pagebreak -->

## Correct answer

React's count variable is a snapshot of state for the current render. Calling setCount() doesn't change that snapshot.

```tsx
setCount(0 + 1);
setCount(0 + 1);
setCount(0 + 1);
```

React batches these updates, and the displayed value becomes 1.

One solution is:

```tsx
setCount(count + 3);
```

There's another important approach: functional state updates.

```tsx
setCount(prev => prev + 1);
setCount(prev => prev + 1);
setCount(prev => prev + 1);
```

This also produces 3, because React processes the queued updater functions in order, passing each the result of the previous update. Functional updates are especially useful when the next state depends on the previous state.
