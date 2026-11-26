# Question 8 — React useEffect and race conditions

**React / Async Data Fetching — Senior**

```tsx
import { useEffect, useState } from "react";

function UserProfile({ userId }: { userId: number }) {
  const [user, setUser] = useState<any>(null);

  useEffect(() => {
    fetch(`/api/users/${userId}`)
      .then(response => response.json())
      .then(data => setUser(data));
  }, [userId]);

  return <div>{user?.name ?? "Loading..."}</div>;
}
```

Imagine the following sequence:

| Time | Event |
|---|---|
| 0 ms | userId = 1 — request A starts |
| 50 ms | userId = 2 — request B starts |
| 100 ms | Request B completes |
| 300 ms | Request A completes |

1. Which user's name will be displayed after 300 ms?

<textarea name="q8-1" rows="3" placeholder="Your answer"></textarea>

2. What is the underlying problem?

<textarea name="q8-2" rows="3" placeholder="Your answer"></textarea>

3. How would you fix it?

<textarea name="q8-3" rows="3" placeholder="Your answer"></textarea>


Bonus: What happens if the component unmounts while a request is still running?

<textarea name="q8-Bonus" rows="3" placeholder="Your answer"></textarea>


<!-- pagebreak -->

## Correct answer

After 300 ms, the component displays User A, even though the current userId is 2. This is a classic race condition caused by stale asynchronous responses.

Request B finishes at 100 ms and calls setUser(B). Request A finishes at 300 ms and calls setUser(A), overwriting the newer result. React doesn't automatically know that request A is obsolete.

### How to fix it

Use AbortController to cancel the previous request when userId changes or the component unmounts.

```tsx
useEffect(() => {
  const controller = new AbortController();

  async function loadUser() {
    try {
      const response = await fetch(
        `/api/users/${userId}`,
        { signal: controller.signal }
      );

      if (!response.ok) {
        throw new Error(`HTTP ${response.status}`);
      }

      const data = await response.json();

      if (!controller.signal.aborted) {
        setUser(data);
      }
    } catch (error) {
      if (!controller.signal.aborted) {
        console.error(error);
      }
    }
  }

  loadUser();

  return () => controller.abort();
}, [userId]);
```

The cleanup function runs before the effect is executed again for a changed dependency, and on unmount. The abort signal prevents obsolete requests from updating the component. For production code, we'd also manage loading and error states.
