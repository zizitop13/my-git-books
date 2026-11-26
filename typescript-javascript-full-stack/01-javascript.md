# JavaScript foundations

JavaScript primitives are values; object references are values too. Assignment does not clone objects. Spread is shallow. const prevents rebinding, not mutation. Prefer === to avoid coercion; ?? preserves zero and false unlike ||.

## Closures and prototypes
let and const are block scoped; var is function scoped. Closures retain lexical bindings. A loop with let gives each iteration a separate binding; with var, delayed callbacks observe the final value. Ordinary functions receive this from their call site, arrows capture lexical this. Property lookup follows the prototype chain.

## Practice
Explain why a detached method loses its receiver, why numeric sorting needs a comparator, and when Map is preferable to a plain object.
