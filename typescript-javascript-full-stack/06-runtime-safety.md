# Question 6 — TypeScript runtime safety

**Backend / TypeScript — Senior**

```typescript
interface User {
  id: number;
  name: string;
}

async function getUser(): Promise<User> {
  const response = await fetch(
    "https://api.example.com/user"
  );

  const user = await response.json() as User;

  return user;
}

const user = await getUser();

console.log(user.name.toUpperCase());
```

Imagine the API returns:

```json
{
  "id": "123",
  "name": null
}
```

1. Will TypeScript compile this code?
2. What happens at runtime?
3. How would you make this code type-safe in production?

<!-- pagebreak -->

## Your answer

1. Yes I think
2. Some exception related to custing
3. Use some typescript fancy pattern😅

## Feedback

You're on the right track. **2/3 conceptually.** The missing piece is runtime validation.

Yes, it compiles. as User is a type assertion, not a cast in the Java sense. It tells the TypeScript compiler to treat the value as a User, but doesn't validate or transform anything at runtime.

```text
TypeError: Cannot read properties of null
(reading 'toUpperCase')
```

The id also remains a string. TypeScript doesn't convert it into a number.

### How to solve it in production

A popular solution is Zod.

```typescript
import { z } from "zod";

const UserSchema = z.object({
  id: z.number(),
  name: z.string()
});

type User = z.infer<typeof UserSchema>;

async function getUser(): Promise<User> {
  const response = await fetch(
    "https://api.example.com/user"
  );

  if (!response.ok) {
    throw new Error(`HTTP ${response.status}`);
  }

  const data: unknown = await response.json();

  return UserSchema.parse(data);
}
```

If id is a string or name is null, Zod throws a validation error before invalid data reaches the application. You can also use safeParse() to handle validation failures without exceptions.

| Java | TypeScript |
|---|---|
| Jackson deserialization with validation | Zod schema parsing |
| Java class/interface | TypeScript interface/type |
| Runtime class information | Interfaces erased at runtime |
| Bean Validation | Zod validation rules |

Jackson deserialization alone doesn't necessarily reject every invalid value. Strict validation must be configured where appropriate.
