# TypeScript type system

## Structural typing
An Admin with id, name and permissions is assignable to User with id and name. Passing a fresh literal with permissions directly to a User parameter can fail excess property checking. Unlike Java nominal interfaces, explicit implementation is unnecessary.

```ts
function getProperty<T, K extends keyof T>(obj: T, key: K): T[K] {
  return obj[key];
}
const user = { id: 42, name: "Alice", active: true };
```
getProperty(user, "name") returns string; active returns boolean; email fails compilation. keyof defines the valid keys and T[K] the selected value type.

## Narrowing
```ts
type Result<T> =
  | { ok: true; value: T }
  | { ok: false; error: string };
```
The discriminant enables narrowing. unknown requires checks; any bypasses them; never represents impossible values. Partial, Pick, Omit, Record and Readonly transform types. Prefer strict mode and consider noUncheckedIndexedAccess.

## Runtime validation
Assertions do not transform JSON. A null name still throws TypeError when toUpperCase is called. Validate unknown input at boundaries:
```ts
import { z } from "zod";
const User = z.object({ id: z.number(), name: z.string() });
type User = z.infer<typeof User>;
const validated = User.parse(await response.json());
```
Check HTTP status first. Use safeParse for explicit error handling. Neither as nor satisfies performs runtime validation.
