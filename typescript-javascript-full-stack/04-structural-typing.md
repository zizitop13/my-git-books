# Question 4 — TypeScript type system

**TypeScript / Structural Typing — Medium**

```typescript
interface User {
  id: number;
  name: string;
}

interface Admin {
  id: number;
  name: string;
  permissions: string[];
}

function printUser(user: User) {
  console.log(user.name);
}

const admin: Admin = {
  id: 1,
  name: "Alice",
  permissions: ["DELETE"]
};

printUser(admin);

// What about this?
printUser({
  id: 1,
  name: "Bob",
  permissions: ["DELETE"]
});
```

1. Will printUser(admin) compile?
2. Will the second printUser(...) call compile?
3. Why does TypeScript behave this way?
4. How does this differ from Java's interface system?

<!-- pagebreak -->

## Your answer

1. Yes
2. No

## Feedback

Both answers are correct! **2/2.**

TypeScript uses structural typing. An object is compatible with an interface if it has the required properties with compatible types. Admin contains everything required by User even though Admin doesn't explicitly extend it.

TypeScript performs an additional check on fresh object literals called excess property checking. The permissions property isn't declared in User.

However, this works:

```typescript
const bob = {
  id: 1,
  name: "Bob",
  permissions: ["DELETE"]
};

printUser(bob); // OK
```

### Comparison with Java

Java primarily uses nominal typing: a class must explicitly implement an interface.

```java
interface User {
    String name();
}

record Admin(String name) {
    // Doesn't implement User
}
```

Even though Admin has a compatible name() method, Java won't treat it as User. TypeScript checks the shape instead.

**TypeScript interfaces disappear at runtime.** They don't provide runtime validation of JSON or API responses.
