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
2. Why doesn't React.memo() prevent it?
3. How would you fix the unnecessary re-render?

Bonus: Would you always apply this optimization in production?

Try all three. This is a common distinction between understanding React's API and understanding its rendering model.

*Not answered yet in our chat.*
