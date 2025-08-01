# Curry Function
Currying is the technique of converting a function that takes multiple arguments into a sequence of functions that each takes a single argument.

```javascript
/**
 * @param {Function} func
 * @return {Function}
 */
export default function curry(func) {
  return function curried(...args){
    // 1. Check if we have enough arguments.
    if(args.length >= func.length){
      // 2. If so, call the original function with all arguments.
      return func.apply(this, args);
    } else {
      // 3. If not, return a new arrow function that waits for the rest.
      return (...nextArgs) => { //backpack
        // in the function call the curried with combined args
        return curried.apply(this, [...args, ...nextArgs]);
      }
    }
  }
}
```
This implementation uses an arrow function in its recursive step to correctly preserve the `this` context across multiple calls.

---

### Note:

1.  **Closure** is a combination of a function bundled together with references to its lexical environment (surrounding state). We can keep a record of the curried function arguments so far via closures.
    => the inner function is the "backpack" => It "remembers" the 'args' from its parent

2.  Note that the inner function needs to be defined using **arrow functions** to preserve the same lexical `this`. (see Debounce)

3.  **The Spread Syntax (...)**
    You need to "flatten" or "spread" the contents of both `args` and `nextArgs` into a single, new array. The spread syntax (`...`) is perfect for this.

    When you write:
    `func.apply(this, [arg1, arg2, arg3])`

    JavaScript essentially translates this into:
    `func.call(this, arg1, arg2, arg3)`
