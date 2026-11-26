# Question 5 — TypeScript generics and type safety

**TypeScript / Generics — Medium–Hard**

```typescript
function getProperty<T, K extends keyof T>(
  obj: T,
  key: K
): T[K] {
  return obj[key];
}

const user = {
  id: 42,
  name: "Alice",
  active: true
};

const a = getProperty(user, "name");
const b = getProperty(user, "active");
const c = getProperty(user, "email");
```

1. What are the inferred types of a and b?

<textarea name="q5-1" rows="3" placeholder="Your answer"></textarea>

2. Will the assignment to c compile?

<textarea name="q5-2" rows="3" placeholder="Your answer"></textarea>

3. What do keyof T and T[K] mean?

<textarea name="q5-3" rows="3" placeholder="Your answer"></textarea>


<!-- pagebreak -->

## Correct answer

The inferred types and compilation result are:

| Variable | Correct result |
|---|---|
| a | string |
| b | boolean |
| c | Compilation error |

TypeScript uses lowercase string and boolean for primitive types.

T is the object's type. keyof T is the union of its property names: "id" | "name" | "active". K extends keyof T restricts K to those property names. T[K] represents the type of the property selected by K.

```typescript
getProperty(user, "name");   // string
getProperty(user, "active"); // boolean
getProperty(user, "email");  // Compile error
```

If you bypassed the type checker and accessed user["email"] at runtime, the result would indeed be undefined. This is an important difference between compile-time type safety and JavaScript runtime behavior.
