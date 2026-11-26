# Question 9 — React rendering and memoization

**React / Performance — Senior**

```tsx
import { useState, memo } from "react";

const Child = memo(function Child({
  onClick
}: {
  onClick: () => void;
}) {
  console.log("Child rendered");

  return <button onClick={onClick}>Click</button>;
});

function Parent() {
  const [count, setCount] = useState(0);

  const handleClick = () => {
    console.log("Clicked");
  };

  return (
    <div>
      <p>Count: {count}</p>
      <Child onClick={handleClick} />
      <button onClick={() => setCount(count + 1)}>
        Increment
      </button>
    </div>
  );
}
```

1. When the Increment button is clicked, does Child re-render?

<textarea name="q9-1" rows="3" placeholder="Your answer"></textarea>

2. Why doesn't React.memo() prevent it?

<textarea name="q9-2" rows="3" placeholder="Your answer"></textarea>

3. How would you fix the unnecessary re-render?

<textarea name="q9-3" rows="3" placeholder="Your answer"></textarea>


Bonus: Would you always apply this optimization in production?

<textarea name="q9-Bonus" rows="3" placeholder="Your answer"></textarea>


<!-- pagebreak -->

## Correct answer

1. Yes. Child re-renders when Parent renders because handleClick is a new function reference.
2. React.memo compares each prop using Object.is by default. The callback reference changes even though its behavior is identical.
3. Use useCallback with the appropriate dependencies:

```tsx
import { useCallback } from "react";

const handleClick = useCallback(() => {
  console.log("Clicked");
}, []);
```

The stable callback lets memo skip a parent-driven render when other props are unchanged. Local state and context changes can still trigger renders.

**Bonus:** No. Measure first; memoization adds overhead and complexity. Use it when the avoided rendering work is meaningful.
